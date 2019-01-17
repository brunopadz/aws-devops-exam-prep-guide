# Bootstrapping

* Bootstrapping is a process during which you start a base image / AMI and via automation build on it to create a more complex object.

## Scenario 1

Imagine you need to setup an Ubuntu server to a certain application. You start with a  clean install of Ubuntu.

The next step after provisioning the instance is to install patches and hotfixes to the OS and after that you may need to install additional components that are needed by the application. 

After all these steps, you can deploy your application to the server and create an AMI.

This process can be achived using cloud-init.

## AMI approach

* With an AMI based approach, you'd need a large number of AMIs. Each one for every version of your application.
* To minimize the number of AMIs, you could use a base AMI for the server you'd want to provision and use Chef from OpsWorks to deal with configuration.
