# Immutable Infrastructure

* Pets x Cattle 
* Traditional architecture treats servers as precious (pets)
* Immutable infrastructure is the practice of replacing infrastructure instead of upgrading or repairing faulty components.
* Immutable infrastructure treats servers as throwaway objetcts, if a failure is detected you remove the server and create a new one from a base AMI (cattle)

## What does it mean from AWS architectural perspective?

* Treat servers as unchageable objets.
* Don't diagnose and fix - Throw away and re-create.
* Nothing is bootstrapped, nothing is changed, and image for every version, pre-created and ready to use.

## Scenario 1

We need to provision a NodeJS app

* We first create a working server using our CI/CD process
* Create a base AMI from the working instance
* From this AMI, we create 4 nodes. Each node is an immutable server instance
