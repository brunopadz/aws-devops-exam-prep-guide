# CloudWatch

## What's CloudWatch

* It's metric gathering services, a monitoring/alerting service and a graphing service
* There pre-defined metrics for services, such as: EC2, RDS, EBS
* Metrics fall under different namespaces so they aren't accidentally aggregated
* Metric retention schedules:
    * 1 minute datapoints are available for 15 days
    * 5 minute datapoints are available for 63 days
    * 1 hour datapoints are available for 455 days

## Custom Metrics

* It's possible to push custom metrics to CloudWatch using CloudWatch API and aws cli

`aws cloudwatch put-metric-data --metric-name PageViewCount --namespace MyService --value 2 --timestamp 2016-10-20T12:00:00.000Z`

## CloudWatch Alarms

* CloudWatch Alarms are used to initiate actions on your behalf based on parameters you specify.
* Actions are sent to:
    * SNS
    * AutoScaling Group
* Alarm period should be equal or greater than the metric frequency
* Alarms can't invoke actions because they are in a state, the state must change
* Alarm actions must be in the same region as the alarm
* You can have up to 5000 alarms per region and per account
* Examples of commands to configure alarms:
    * The command `mon-put-metric-alarm` can be used to create or update an alarm
    * `mon-[enable|disable]-alarm` to enable and disable alarms
    * `mon-describe-alarms` to describe alarms

### Alarm States

* OK
* ALARM
* Insufficient Data

### AutoScale based on metrics

* It's possible to scale an AutoScaling Group based on metrics. It's possible to add and remove instances based on thresholds. 

## CloudWatch Logs

* It monitors existing system, application and custom log files in real time. It's also possible to send existing logs to CloudWatch, create patterns to look for in your logs and alarms based on these patterns.
* There are agents for:
    * Amazon Linux
    * Ubuntu
    * Windows
* The 3 purposes of CloudWatch logs is:
    * Monitor logs from instances in realtime
    * Monitor AWS CloudTrail events
    * Archive log data
* Log Events: A record sent to CloudWatch Logs to be stored.
* Log Streams: Sequence of log events that share the same source.
* Log Groups: Groups of Log Streams. It shares the same retention, monitoring and access control settings.
* Metric Filters: Used to define how a service would extract metric observations from events and turn them into data points for a CloudWatch metric.
* Retention Settings: How long log events are kept in CloudWatch Logs.

## CloudWatch Filters

* Define search patterns to look for in a log
* Can be used to turn logs into metrics and then turn them into graphs and alarms
* Filters **won't** work on existing log data. It will only work after the data was pushed to CloudWatch.
* Metric Filters consists of:
    * Filter Pattern
    * Metric Name
    * Metric Namespace
    * Metric Value

## CloudWatch Events

* Similar to CloudTrail
* Near real-time stream of events
* Route events to Lambda, Kinesis, SNS stream. **It can be multiple targets.**
* Main components:
    * Events: Small pieces of JSON
    * Rules: Match incoming events and route them to one or more targets for processing
    * Targets: Where the events are processed
