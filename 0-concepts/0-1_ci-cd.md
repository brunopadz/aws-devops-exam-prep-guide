# Continuous Integration and Continous Delivery

* Continous Integration (CI) and Continuous Delivery (CD) are two core software development processes.

## Continuous Integration

* It's a process of automating regular code commits followed by an automated build and test process designed to highlight integration issues early. 
* Requires additional tooling and functionality provided by tools like Jenkins, GoCD and Bamboo. These tools offer a way to customize the integration/build/test workflow process.

## Continuous Delivery

* CD takes the form of a workflow based process which accepts a tested software build payload from a CI server/tool. 
* The CD server automates the deployment inta a working QA, pre-production or production environment.
* Most of CI tools incorporate functionality allowing continuous delivery.
* AWS CodeDeploy and AWS CodePipeline provide CI/CD services.
* Services like Elastic Beanstalk and CloudFormation provide functionality which can be used by CI/CD tools.

