# AWS DevOps Engineer Professional Preparation Guide

> ⚠️ This preparation guide is based on **my studies** taking courses from A Cloud Guru, Linux Academy and some videos at Safari Online (O'Reilly). ️⚠️

## About the exam

* Multiple choice and multiple-answer questions
* 170 minutes to complete the exam
* Exam registration fee is USD 300

| Domain  | % of examination |
| ------------- | ------------- |
| 1: Continuous Delivery and Process Automation  | 55%  |
| 2: Monitoring, Metrics and Logging  | 20%  |
| 3: Security, Governance and Validation  | 10%  |
| 4: High Availability and Elasticity  | 15%  |

* [More info](https://d1.awsstatic.com/training-and-certification/docs-devops-pro/AWS_certified_devops_engineer_professional_blueprint.pdf)

## About the guide

This guide will take you through the main concepts and topics to pass the exam.

Feel free to contribute (translating, adding more content or fixing typos)! :)

## Index

### 0. Concepts
* [CI / CD](0-concepts/core.md#continuous-integration-and-continous-delivery)
* [Deployment Types](0-concepts/core.md#deployment-types)
* [A/B Testing](0-concepts/core.md#ab-testing)
* [Bootstrapping](0-concepts/core.md#bootstrapping)
* [Immutable Infrastructure](0-concepts/core.md#immutable-infrastructure)
* [Containers](0-concepts/core.md#containers)

### 1. Continuous Delivery and Process Automation
* [CloudFormation](1-ci_cd_automation/cloudformation.md#cloudformation)
    * [What's CloudFormation?](1-ci_cd_automation/cloudformation.md#whats-cloudformation)
    * [CloudFormation Structure](1-ci_cd_automation/cloudformation.md#cloudformation-structure)
    * [Stack Creation](1-ci_cd_automation/cloudformation.md#stack-creation)
    * [Stack Updates](1-ci_cd_automation/cloudformation.md#stack-updates)
    * [Resource Deletion Policies](1-ci_cd_automation/cloudformation.md#resource-deletion-policies)
    * [Nesting](1-ci_cd_automation/cloudformation.md#cloudformation-nesting)
    * Wait Conditions and Handlers (to review)
    * Custom Resources (to review)
* [OpsWorks](1-ci_cd_automation/opsworks.md#opsworks)
    * [What's OpsWorks](/1-ci_cd_automation/opsworks.md#whats-opsworks)
    * [Structure](/1-ci_cd_automation/opsworks.md#structure)
    * [Stacks and Layers](1-ci_cd_automation/opsworks.md#stacks-and-layers)
    * [Lifecycle Events](1-ci_cd_automation/opsworks.md#lifecycle-events)
    * [Instances](/1-ci_cd_automation/opsworks.md#instances)
    * [Applications](1-ci_cd_automation/opsworks.md#applications)
    * [create-deployment command](1-ci_cd_automation/opsworks.md#create-deployment-command)
    * Berkshelf and Databags
    * [Auto-healing](1-ci_cd_automation/opsworks.md#auto-healing)
* [Elastic Beanstalk](1-ci_cd_automation/beanstalk.md)
    * [What's Elastic Beanstalk](1-ci_cd_automation/beanstalk.md#whats-elastic-beanstalk)
    * [Components](1-ci_cd_automation/beanstalk.md#components)
    * [Extending Elastic Beanstalk with ebextensions](1-ci_cd_automation/beanstalk.md#ebextensions)
    * [Docker](1-ci_cd_automation/beanstalk.md#docker)

### 2. Monitoring, Metrics and Logging
* [CloudWatch](2-monitoring_metrics_logging/cloudwatch.md#cloudwatch)
    * [What's CloudWatch](2-monitoring_metrics_logging/cloudwatch.md#whats-cloudwatch)
    * [Custom Metrics](2-monitoring_metrics_logging/cloudwatch.md#custom-metrics)
    * [CloudWatch Alarms](2-monitoring_metrics_logging/cloudwatch.md#cloudwatch-alarms)
    * [CloudWatch Logs](2-monitoring_metrics_logging/cloudwatch.md#cloudwatch-logs)
    * [CloudWatch Filters](2-monitoring_metrics_logging/cloudwatch.md#cloudwatch-filters)
    * [CloudWatch Events](2-monitoring_metrics_logging/cloudwatch.md#cloudwatch-events)

* [CloudTrail](2-monitoring_metrics_logging/cloudtrail.md)

### 3. Security, Governance and Validation

* [Delegation and Federation](3-security/delegation-federation.md)
    * [Corporate Identity Federation](3-security/delegation-federation.md#corporate-identity-federation)
    * [Web Identity Federation](3-security/delegation-federation.md#web-identity-federation)

### 4. HA and Elasticity

### 5. Operations

### 6. Additional tips and links
