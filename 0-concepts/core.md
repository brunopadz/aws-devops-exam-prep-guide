# Core Concepts

## Continuous Integration and Continous Delivery

* Continous Integration (CI) and Continuous Delivery (CD) are two core software development processes.

### Continuous Integration

* It's a process of automating regular code commits followed by an automated build and test process designed to highlight integration issues early. 
* Requires additional tooling and functionality provided by tools like Jenkins, GoCD and Bamboo. These tools offer a way to customize the integration/build/test workflow process.

### Continuous Delivery

* CD takes the form of a workflow based process which accepts a tested software build payload from a CI server/tool. 
* The CD server automates the deployment inta a working QA, pre-production or production environment.
* Most of CI tools incorporate functionality allowing continuous delivery.
* AWS CodeDeploy and AWS CodePipeline provide CI/CD services.
* Services like Elastic Beanstalk and CloudFormation provide functionality which can be used by CI/CD tools.


## Deployment Types

### Single Target Deployment

* It's NOT recommended these days.
* It's used for small development projects, legacy or where there's not a HA infrastructure involved.
* Generally causes an outage.
* Limited rollback options.

### All-at-Once Deployment

* Deployment happens in one step, as with single target deployment. But with this method, the destination is multiple targets.
* More complicated than single target. 
* Shares the negatives of single target.

### Minimum in-service Deployment

* Deployment happens in multiple stages and to as many targets as possible while maintaining the minimum in-service targets.
* A few moving parts, orchestration and health checks are required.
* Allows automated testing, deployment targets are assessed and testes prior to continuing.
* No downtime, because there's always instances in-service.
* Quicker and less stages than rolling deployment.

### Rolling Deployment

* Deployment happens in multiple stages and the number of targets per stage is user-defined.
* Moving parts, orchestration and health checks are also required.
* Can be the least efficient deployment time based on time-taken.
* Allows automated testing, deployment targets are assessed and tested prior to continuing.
* No downtime - Assuming the number of targets per run isn't large enough to impact the application.
* Can be paused, allowing multi-version testing.

### Blue Green Deployment

* Requires advanced orchestration tooling.
* Carries significant cost - Maintaining 2 environments for the duration of deployments.
* Deployment process is faster - Entire environment is deployed all at once. It's not used to feature test, it's generally used to perform full and final updates.
* Cutover/migration and rollback is clean and controlled (via DNS change)
* Health and performance of entire "green" environment can be tested prior to cutover.
* It's possible to use CloudFormation to automate the process.
* Not to be confused with A/B testing.

### Summary

* Know the pros and cons of each deployment type.
* Know when each should be used and when not.
* Know the limitations of each, how quick is deployment, how quick is to rollback.
* Know how each deployment type impacts your apps.
* Know which AWS Services support which deployment type.


## A/B Testing

* Feature test is not used when doing Blue Green Deployments. A/B Testing solves this.
* A/B Testing sends a percentage of traffic to Blue environment and a percentage to Green environment.
* It separates different versions of your code, which may have different reliability or performance.
* It allows a gradual performance/stability/health analysis.
* It allows new features to be tested with a subset of visitors
* If bugs are detected, rollback can be done quickly and there'll be a minimal damage.

### Scenario 1

* Environment A starts with 100% of traffic directed at the current (Blue) environment.
* When the deployment completes, the distribution of traffic is changed to direct a small amount of users at the new environment (Green).
* As confidence grows, the distribution is reversed.
* Finally 100% of traffic is directed at the Green environment and the confidence reaches previous levels, the Blue environment can be decomissioned.

### How it's performed on AWS

* Using Route53:
    * Two records are created, one pointing to A and the other pointing to B.
    * This is called Round-Robing with a 50/50 request distribution.
    * Weighted Round-Robing is an advanced feature where you can specify a weight for each environment, which allows a granular and adjustable distribution such as: 100%/0%, 70%/30%, 50%/50% or anything in between.
    * **IMPORTANT**: As it's based on DNS, caching and other DNS related issues can impact the overall accuracy of this technique.


## Bootstrapping

* Bootstrapping is a process during which you start a base image / AMI and via automation build on it to create a more complex object.

### Scenario 1

Imagine you need to setup an Ubuntu server to a certain application. You start with a  clean install of Ubuntu.

The next step after provisioning the instance is to install patches and hotfixes to the OS and after that you may need to install additional components that are needed by the application. 

After all these steps, you can deploy your application to the server and create an AMI.

This process can be achived using cloud-init.

### AMI approach

* With an AMI based approach, you'd need a large number of AMIs. Each one for every version of your application.
* To minimize the number of AMIs, you could use a base AMI for the server you'd want to provision and use Chef from OpsWorks to deal with configuration.

## Immutable Infrastructure

* Pets x Cattle 
* Traditional architecture treats servers as precious (pets)
* Immutable infrastructure is the practice of replacing infrastructure instead of upgrading or repairing faulty components.
* Immutable infrastructure treats servers as throwaway objetcts, if a failure is detected you remove the server and create a new one from a base AMI (cattle)

### What does it mean from AWS architectural perspective?

* Treat servers as unchageable objets.
* Don't diagnose and fix - Throw away and re-create.
* Nothing is bootstrapped, nothing is changed, and image for every version, pre-created and ready to use.

### Scenario 1

We need to provision a NodeJS app

* We first create a working server using our CI/CD process
* Create a base AMI from the working instance
* From this AMI, we create 4 nodes. Each node is an immutable server instance


## Containers

### Virtualization vs Containerization

* A traditional VM contains the entire OS
* Traditional virtualization has density compromises (need image to example)
* Containers achieve higher density and improved compatibility by removing the per container OS (image here)

### Container benefits

* There's no dependancy hell. Don't worry with dependencies and library versions.
* Consistent progression from Dev env to Prod env.
* Isolation. Performance or stability issues with App A in container A won't impact App B in container B.
* Resource scheduling at the micro level. Performance can be more accurately controled.
* Code portability.
* Microservices.

### Docker Components 

* Docker Image - Used to build containers
* Docker Container - Holds everything that's needed for your app to run
* Layers / UFS - Each image consists in a series of layers. 
* Dockerfile - A set of instructions stored in a file to create an image.
* Docker Engine 
* Docker Client
* Docker Registry
