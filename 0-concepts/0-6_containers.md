# Containers

## Virtualization vs Containerization

* A traditional VM contains the entire OS
* Traditional virtualization has density compromises (need image to example)
* Containers achieve higher density and improved compatibility by removing the per container OS (image here)

## Container benefits

* There's no dependancy hell. Don't worry with dependencies and library versions.
* Consistent progression from Dev env to Prod env.
* Isolation. Performance or stability issues with App A in container A won't impact App B in container B.
* Resource scheduling at the micro level. Performance can be more accurately controled.
* Code portability.
* Microservices.

## Docker Components 

* Docker Image - Used to build containers
* Docker Container - Holds everything that's needed for your app to run
* Layers / UFS - Each image consists in a series of layers. 
* Dockerfile - A set of instructions stored in a file to create an image.
* Docker Engine 
* Docker Client
* Docker Registry
