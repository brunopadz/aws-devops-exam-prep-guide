# Deployment Types

## Single Target Deployment

* It's NOT recommended these days.
* It's used for small development projects, legacy or where there's not a HA infrastructure involved.
* Generally causes an outage.
* Limited rollback options.

## All-at-Once Deployment

* Deployment happens in one step, as with single target deployment. But with this method, the destination is multiple targets.
* More complicated than single target. 
* Shares the negatives of single target.

## Minimum in-service Deployment

* Deployment happens in multiple stages and to as many targets as possible while maintaining the minimum in-service targets.
* A few moving parts, orchestration and health checks are required.
* Allows automated testing, deployment targets are assessed and testes prior to continuing.
* No downtime, because there's always instances in-service.
* Quicker and less stages than rolling deployment.

## Rolling Deployment

* Deployment happens in multiple stages and the number of targets per stage is user-defined.
* Moving parts, orchestration and health checks are also required.
* Can be the least efficient deployment time based on time-taken.
* Allows automated testing, deployment targets are assessed and tested prior to continuing.
* No downtime - Assuming the number of targets per run isn't large enough to impact the application.
* Can be paused, allowing multi-version testing.

## Blue Green Deployment

* Requires advanced orchestration tooling.
* Carries significant cost - Maintaining 2 environments for the duration of deployments.
* Deployment process is faster - Entire environment is deployed all at once. It's not used to feature test, it's generally used to perform full and final updates.
* Cutover/migration and rollback is clean and controlled (via DNS change)
* Health and performance of entire "green" environment can be tested prior to cutover.
* It's possible to use CloudFormation to automate the process.
* Not to be confused with A/B testing.

## Summary

* Know the pros and cons of each deployment type.
* Know when each should be used and when not.
* Know the limitations of each, how quick is deployment, how quick is to rollback.
* Know how each deployment type impacts your apps.
* Know which AWS Services support which deployment type.
