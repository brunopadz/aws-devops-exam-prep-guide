# CloudTrail

## What's CloudTrail

* CloudTrail records all API calls made in your account to enable security analysis to track changes and provide compliance auditing.
* These logs contain:
    * Identity of who made the API call
    * Time
    * Source IP
    * Request parameters
    * Response
* There are 2 types of trails:
    * All regions
    * One region
* The log files are stored in S3 using SSE
* Logs are delivered within 15 minutes of an API call being made
* New log files are published every 5 minutes
* The field `invokedBy` can be used to see what triggered the API call
