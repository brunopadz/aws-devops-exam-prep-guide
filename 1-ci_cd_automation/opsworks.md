# OpsWorks

## What's OpsWorks

* OpsWorks is the AWS implementation of Chef configuration management and automation system. It allows provisioning of infrastructure, but abstracts some of the details.

## Structure

* OpsWorks consists in:
    * Stack: Could be an entire application or environment
    * Layer: Set of shared of functionality/architecture which is applied for a group of components. It defines what packages are installed, how they're configured and how the applications are deployed.
    * Instance: Units of compute, such as an EC2 instance. It defines the the resource's basic configuration, such as OS and instance size.
    * Application
* OpsWorks is composed of 2 pieces of technology. 
    * OpsWorks Agent (Chef): Responsible for configuration of machines/instances/resources
    * OpsWorks Automation Engine: Creates, updates and deletes various AWS components. 
* OpsWorks and Chef are declarative desired state engines. Within Chef/OpsWorks there are resources which allows to install packages, control services (start/stop/restart) and update configuration files.
* Recipes tell OpsWorks what you want the end result will be
* Cookbooks contain recipes and all associated data to support them

Recipe example:

```ruby
packge "httpd" do
    action :install
end

service "httpd" do
    action [:enable, :start]
    supports :restart => :true
end

template "/var/www/html/index.html" do
    source "index.html.erb"
    owner "apache"
    group "apache"
    mode "0644"
    notifies :restart, "service[httpd]"
end
```

## Stacks and Layers

### Stacks

* It represents a set of instances that you want to manage collectively, typically because they have a common purpose such as serving PHP applications. In addition to serving as a container, a stack handles tasks that apply to the group of instances as a whole, such as managing applications and cookbooks.
* **IMPORTANT**: There are elements that can be changed later when creating a stack and there are elements that can't.
* Instances launched using OpsWorks needs internet access, or via IGW, NGW or NAT Instance.
* The default OS for instances can be changed later, but it doesn't update existing instances.
* It's possible to use custom Chef cookbooks. The sources available are: Git, S3 and HTTP.

### Layers

* Every stack contains one or more layers, each of which represents a stack component.
* Each layer must have at least one instance and an instance can be assigned to multiple layers.
* It's possible to register existing resources such as: Elastic IPs, Volumes and RDS
* When adding layers, there are 3 high level types:
    * OpsWorks: Allows rich feature sets via recipes
    * ECS: Allows integration with ECS
    * RDS: Allows integration with an existing RDS instance
    * **IMPORTANT**: 
        * An RDS instance can only be associated with one OpsWorks Stack
        * A stack clone operation doesn't copy an existing RDS instance

## Lifecycle Events

* OpsWorks has 5 events which occur during the lifetime of an instance. Some events occur once and others multiple times. These events are:
    * Setup: This event is executed after the instance has finished booting. It performs initial base setep.
    * Configure: This event is executed on all instances can be executed in 3 scenarios;
        * When an instance goes online
        * Associate or disassociate and Elastic IP
        * Attach or detach an Elastic Load Balancer to a layer
    * Deploy: This event is executed when the deploy command is ran. It runs recipes that deploy the application to instances.
    * Undeploy: This event is executed when an application is deleted.
    * Shutdown: Runs recipes before the instance is terminated, allowing cleanup.
* Events can be run manually by the "Stack run" functionality.
* Each layer has its own recipes for each event.

## Instances

* Instances can be added in 2 locations, the layer or the stack instances menu.
* There are 3 types of instances:
    * 24/7: Provisioned manually. Start and Stop process are also manual. Good for small stack deployments. During its creation, it's possible to override the following options:
        * Subnet
        * SSH Key
        * Operating System
        * Root device type
        * Volume type
    * Time-based: Initially provisioned and configured to start and stop at certain times during the day.
    * Load-based: Similar to time-based, but responds to certain loads criteria. For these instance type, it's necessary to setup a per-layer scaling configuration.

## Applications

* In the context of OpsWorks, Applications are objects which represents the application and associated metadata.
* Within the Application object there is the following properties:
    * Application Name
    * Document Root
    * Data Source (RDS/None)
    * Application Source (Git/HTTP/S3)
    * Environment Variables
    * Domain Names
    * SSL Enable and Settings
* Each time an application is deployed, OpsWorks keeps 4 previous versions in addition to the current version.

### Scenario: Steps performed while deploying an application

1. It executes the deploy recipes on the instances targeted by the command. 
2. The `application-id` is passed to the command.
3. Application Parameters are passed into the Chef environment within Databags
> Databags are JSON objects that contain data. 
4. The deploy recipe accesses the application source information from the Databag and pulls the application on the instance.

## `create-deployment` command

* Creates and manipulates application deployments
* Allows stack level commands to be executed against the stack

### Syntax examples

* Main syntax `aws opsworks --region sa-east-1 create-deployment`
    * `--stack-id`: To specify the stack which the command will be run against
    * `--app-id`: To reference the Application ID. Not used in all cases.
    * `--instance-ids`: To reference the list of instances 
    * `--comment`: User-defined comment string
    * `--custom-json`: Allows custom JSON that can be referenced by recipes
    * `--generate-cli-skeleton`: Generates JSON skeleton which can be updated manually and used with `--cli-input-json`
    * `--cli-input-json`
    * `--command`: The command to be executed. Available commands are:
        * `install_dependencies`
        * `update_dependencies`
        * `update_custom_cookbooks`
        * `execute_recipes`
        * `configure`
        * `setup`
        * `deploy`
        * `rollback`
        * `start`
        * `stop`
        * `restart`
        * `undeploy`

### Scenario 1: Deployments

Consider the following scenario: There's already an application deployed and there's a need to deploy new versions, undeploy and rollback.

To deploy:

`aws opsworks --region sa-east-1 create-deployment --stack-id 12341234-1234-1234-1234-123412341234 --app-id 12341234-1234-1234-1234-123412341234 --command "{\"Name\": \"deploy\"}"`

To undeploy:

`aws opsworks --region sa-east-1 create-deployment --stack-id 12341234-1234-1234-1234-123412341234 --app-id 12341234-1234-1234-1234-123412341234 --command "{\"Name\": \"undeploy\"}"`

To rollback:

`aws opsworks --region sa-east-1 create-deployment --stack-id 12341234-1234-1234-1234-123412341234 --app-id 12341234-1234-1234-1234-123412341234 --command "{\"Name\": \"rollback\"}"`

### Stack commands

* `update_custom_cookbooks`: Downloads again the custom cookbooks from the repository. 
* `execute_recipes`: Run the recipes
* `setup`
* `configure`
* `update_dependencies`
* `upgrade_operating_system`

## BerkShelf and Databags

## Auto-healing

* Each OpsWorks instance has an agent installed. This agent performs a heartbeat style health check with OpsWorks Orchestration Engine.
* If this heartbeat fails for 5 minutes, OpsWorks marks the instance as unhealthy and performs an auto-heal process.
* Actions performed by auto-heal depends on the circumstance under which is initiated.
* For EBS-backed instances the stop/start workflow is:
    1. Online
    2. Stopping
    3. Stopped
    4. Requested
    5. Pending
    6. Booting
    7. Online
* For Instance Store instances the stop/start workflow is:
    1. Online
    2. Shutting down
    3. Requested
    4. Pending
    5. Booting
    6. Online
* Auto-healing won't recover from serious instance corruption or if the agent is damaged.
* Auto-healing isn't a performance response, it's a **failure** response. 

