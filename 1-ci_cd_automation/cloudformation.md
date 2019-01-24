# CloudFormation

## What's CloudFormation?

* It's a building block service designed to provision infrastructure (Infra as Code) with AWS. Can be used directly (users) or indirectly (services). 
* Easy to read/write, since it's JSON.
* Can be versioned.
* There are 3 main terms to understand:
    * Stack - CloudFormation's unit of grouping for infrastructure.
    * Template - A JSON document which gives instructions to CloudFormation on how to act and what to create. It can be used to create or update a stack.
    * Stack Policy - IAM style policy statement which governs what can be changed and by who.

### Simple CloudFormation Workflow

1. Create a template
2. Add the template to CF 
3. Then CF creates the stack defined in the template. It can be a bucket, a RDS instance or a group of resources (limited by 2000 resources)
4. When needed the template can be updated, which also will update the stack
5. Delete the stack

### Template Anatomy

```json
{
    "AWSTemplateFormatVersion": "version date",

    "Description": "JSON String",

    "Parameters": {
        set of parameters
    },

    "Mappings": {
        set of mappings
    },

    "Resources": {
        set of resources
    },

    "Outputs": {
        set of outputs
    } 
}
```

* Parameters - Allow the passing of variables into a template via UI, CLI or API. Examples: Instance sizes, AMI IDs, SSH Keys and much more.
* Mappings - Allow processing of hashes (array of key value pairs) by the `cfnTemplate`.
* Resources - Where the resources to be created are declared. **The only mandatory section of the template**
* Outputs - Results from the templates

### What's capable of?

* CloudFormation can have conditional elements to resources, or the whole resource can be conditional.
* It can run scripts and expand files within instances
* Each stack has a Stack ID - Each stack and associated resources are unique
* The events during stack creation, update and deletion can be configured.

### Where to use it

There are many applications for CloudFormation, for example:

* To create a repeatable patterned environment
* To run automated testing for CI/CD environments, create a dedicated, clean environment, inject code, run testing, produce results and delete the test environment.
* To be able to define and environment once and have it deployed to any region of AWS without reconfiguration.
* To manage infrastructure as code.
* A template should be designed to run as many times as possible and in many regions as possible. **DO NOT USE STATIC NAMES**.

## CloudFormation Structure

The following topic covers the structure of a CloudFormation template

### Parameters

* Offer a way to pass data to a template, like: IP Addresses, Instance Size, SSH Keys
* Each parameter definition can have a number of attributes such as:
    * Type - This could be a String, Number, List, CommaDelimitedList and specific AWS types such as: `AWS::EC2::KeyPair::KeyName` and `AWS::EC2::AvailabilityZone::Name`.
* A parameter definition can also have:
    * Default Value: which becomes the parameter value if none is specified.
    * Allowed Values: which specifies one or more values that can be passed to the parameter
    * Allowed Pattern: RegEx that defines the format the parameter can take
    * Min and MaxValue: To specify lower and upper limits
    * Min and MaxLength: To specify min and max length of the input string

The following template asks for a bucket name:

```json
{ 
    "AWSTemplateFormatVersion": "version date",

    "Parameters": {
        "Bucketname": {
            "Type": "String",
            "Default": "aws-prep-guide",
            "Description": "Enter a bucket name",
            "MinLength": 5,
            "MaxLength": 30
        }
    }
}
```

For further reading, consult the [AWS Docs](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html).

### Resources

* The section where resources to be configured are specified
* Each resource consists of:
    * Logical ID: which is how the resource is referenced by the other sections of the template
    * Type: Which must be specified within the template for each object being created
    * Properties: Configures resource specific elements - some of which may be mandatory or optional, depending on the resource type

With the following template, it's possible to create a S3 bucket without a name. If the bucket name property is not specified this template could be **applied 5 times to create 5 resources**, where the names can be auto-generated, **allowing re-use of the template in a larger scale**.

```json
{
    "Resources": {
        "Bucket": {
            "Type": "AWS::S3::Bucket"
        }
    }
}
```

It's also worth noting that when the stack is deleted, the resources are also deleted.

It's not a good practice to use static usernames and passwords, to solve this problem it's possible to reference Parameters within the Resources using the function **`Ref`**, like the following example:

```json
"Parameters": {
    "DBUser": {
        "Type": "String"
    },
    "DBPassword": {
        "Type": "String"
    }
}

"Resources": {
    "MyDB": {
        "Type": "AWS::RDS::DBInstance",
        "Properties": {
            "AllocatedStorage": "5",
            "DBInstanceClass": "db.t2.micro",
            "Engine": "MySQL",
            "MasterUsername": {"Ref": "DBUser"},
            "MasterUserPassword": {"Ref": "DBPassword"},
            "StorageType": "gp2"
        }
    }
}
```

### Outputs

* Outputs are a way of displaying results of a stack creation. It can be displayed on CLI/UI or used in parent stacks
* A Stack can have many outputs, and each output can be constructed value, parameters references, pseudo parameters or an output from a function such as `fn::GetAtt` or `Ref`
* `fn::GetAtt` is useful as it can provide attributes of resources created in a stack as an output, like Public IP Addresses.

In the next example, a new S3 bucket is created. An output called "BucketName" references the Bucket object and the result is the bucket name as an output.

```json
{
    "Resources": {
        "Bucket": {
            "Type": "AWS::S3::Bucket"
        }
    },

    "Outputs": {
        "BucketName": {
            "Value": { "Ref": "Bucket"}
        }
    }
}
```

### Intrinsic Functions and Conditionals

* Intrinsic functions are built-in functions provided by AWS to help you manage, reference and conditionally act upon resources, situations and inputs to a stack.
* The following intrinsic functions are available:
    * Fn::Base64
    * Fn::FindInMap
    * Fn::GetAtt
    * Fn::GetAZs
    * Fn::Join
    * Fn::Select
    * Ref
* The following conditional functions are available:
    * Fn::And
    * Fn::Equals
    * Fn::If
    * Fn::Not
    * Fn::Or

#### Fn::Base64

* This function accepts plain text and converts to Base64
* It's useful when other elements in a stack need Base64 input such as EC2 user-data

#### Fn::FindInMap

* Performs lookups, it accepts a 'mapping object' one or two keys and return a value
* It's essentially a hash/dictionary lookup function 

#### Fn::GetAtt

* Gets an attribute of a object within the template OR a nested template
* GetAtt allows specific values for an object to be returned, for example EC2 Public or Private IP

#### Fn::GetAZs

* Returns a list of AZs

#### Fn::Join

* Generally used to construct complex values to be used by other resources, functions or property values.
* Can be used to join values together, for example:
`"Fn::Join": [":", ["a", "b", "c"]]` returns "a:b:c"

#### Fn::Select

* Selects a single object from a list of objects.

#### Ref

* Used to referenct other objects or parameter values being created in the template, rather than properties being explicity specified.

## Stack Creation

Steps when creating a stack:

1. Referencing a template: It can be uploaded or referenced from a S3 bucket
2. Syntax check: Ensures the syntax of the template (JSON) is correct
3. Data input: CloudFormation asks for the stack name and any additional info necessary to create the stack, such as parameters.
4. Creation: CloudFormation process the template and start stack creation
5. Resource ordering: CloudFormation reviews all resources and order the resources that must be created first and its dependecies.
6. Resource creation
7. Output generation
8. Stack completion or rollback in case of failures

### DependsOn

* CloudFormation has built-in checking for traditional dependancies, for example, it knows that a VPC is needed before creating subnets.
* It **does not** know specific dependancies from custom infrastructure, it only knows the basic dependancy of AWS services.
* DependsOn influences the automatic dependancy checking of CloudFormation. It allows you to direct CloudFormation on how to handle more complex dependancies. For example:
    * EC2 instances which need a NAT Gateway to be running prior to bootstrapping.
    * EC2 instances which need a RDS instance prior to DB table injection.
* CloudFormation uses DependsOn when deleting/rollback resources as well. It checks for DependsOn when resources are being deleted, for example:
    * An EC2 instance will be deleted before the RDS instance if the EC2 depends on RDS.
* DependsOn is controlled via a `DependsOn` attribute in an object. It references another `cfn` resource, but doesn't use the function.
* It can be a string referencing one object

```json
"EC2": {
    "Type": "AWS::EC2::Instance",
        "DependsOn": "RDS"
}
```

* Or it can be an array

```json
"EC2": {
    "Type": "AWS::EC2::Instance",
        "DependsOn": ["RDS1", "RDS2"]
}
```

## Stack Updates

* The first step to update a stack is check the stack policy. Updates can be prevented as follows:

```json
"Statement": [
    {
        "Effect": "Deny",
        "Action": "Update:*",
        "Principal": "*",
        "Resource": "LogicalResourceId/MyEC2Instance"
    },
    {
        "Effect": "Allow",
        "Action": "Update:*",
        "Principal": "*",
        "Resource": "*"
    }
]
```

* By default, the absence of a stack policy allows all updates.
* Once a stack policy is applied it can't be deleted and **all** objects are protected, where `Update:*` is denied.
* To remove the default deny protection, the policy must be updated with an explicit "Allow" on the resources.
* An update can impact a resource in 4 possible ways:
    * No interruption: There's no service impact. **Example:** Changes in ProvisionedThroughput in DynamoDB.
    * Some interruption: There might be a reboot or any small service impact. **Examples:** Changes in EbsOptimized flag, InstanceType parameter. 
    * Replacement: The change is substantial, the resource will be completely removed and replaced. **Examples:** Changes in AvailabilityZone parameter, ImageId and all changes in LauncConfiguration resources.
    * Delete: The resource will be deleted.
    

## Resource Deletion Policies

* It's a policy or a resource setting which is asociated with each resource in a template to control what happens to these resources when a stack is deleted.
* The policy value can be on of:
    * Delete (Default option)
    * Retain
    * Snapshot
* DeletionPolicy must be set at the top level of resource not inside the object properties

```json
"myS3Bucket": {
    "Type": "AWS::S3::Bucket",
    "DeletionPolicy": "Retain" || "Delete" || "Snapshot"
}
```

### Delete value

* Resources with a `delete` deletion policy are removed along with the stack upon a delete operation.
* Useful in Test environments, CI/CD/QA workflows and immutable environments

### Retain value

* The opposite of delete. Objects still exists after stack deletion.
* Infrastructure can begin to sprawl if not tied to a stack lifecycle. It's common with traditional infrastructure. For example:
    * Windows Server Platforms
    * SQL Server, Exchange and File Servers
    * Non immutable infrastructure

### Snapshot

* It takes a snapshot before deleting the resource.
* Generally used with data processing workloads.
* It's a restricted policy, it's available for a small number of resources including:
    * EC2 Volumes
    * RDS DB Clusters
    * RDS DB Instances
    * Redshift Clusters

## CloudFormation Nesting

* Nesting allows a potentially huge set of infrastructure to be split over multiple templates. **IMPORTANT:** Templates/stacks has size limits.
    * 460Kb template limit (hosted on S3)
    * 200 resources limit in one template/stack
    * 100 mappings, 60 parameters and 60 output limit per template/stack
* Nesting allows more effective Infrastructe as Code reuse.

To create a nested template:

```json
"SQLStack": {
    "Type": "AWS::CloudFormation::Stack"
    "Properties": {
        "TemplateURL": "https://s3.amazon.com/template.json"
        "Parameters": {
            "VPC": {"Fn:GetAtt": ["AD", "OutputsVPC"]}
        }
    }
}
```

## CloudFormation Wait Conditions and Handlers

*to review* 

## CloudFormation Custom Resources

*to review*

* Custom Resource is a resource type within CloudFormation that is backed by SNS or Lambda
* Custom Resources are defined using `"Type: "Custom::ResourceNameHere"` and in the properties the token to stablish a communication with SNS `"ServiceToken": "arn:aws:sns:..."` 
* When a stack is created, deleted or updated a SNS notification is sent to a SNS topic containing the event and the payload. If using Lambda, a Lambda function is invoked and passed an event with the same information.
