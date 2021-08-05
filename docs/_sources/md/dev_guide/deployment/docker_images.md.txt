# Docker Images

To deploy any application to Kubernetes it is needed to be wraped as a container, this can be done by building docker-images. 
Each microservice is packaged as a docker image stored in a image repository provided by [Dockerhub](https://hub.docker.com/u/adisakshya).

As each microservice runs in 2 environments namely `development` and `production` which are inturn two different namespaces in the Kubernetes Cluster, at the source code level - production and development branches are used to diffrentiate them. Each microservice is packed as a docker-image which has 2 tags - one that represents the production package and the second that represents the development package.

- Docker Image for Reminder Service - [adisakshya/reminder-service](https://hub.docker.com/r/adisakshya/reminder-service)
- Docker Image for Event Service - [adisakshya/event-service](https://hub.docker.com/r/adisakshya/event-service)
- Docker Image for Noification Service - [adisakshya/notification-service](https://hub.docker.com/r/adisakshya/notification-service)

The tags for the docker images are named following the format - `{ VERSION }-{ ENVIRONMENT_PREFIX }`. Here `VERSION` is the version of the microservice that has been packed in that docker-image and `ENVIRONMENT_PREFIX` is the short name for a deployment environment.
For example the name of docker image corresponding to the [Event Service](https://hub.docker.com/r/adisakshya/event-service/tags?page=1&ordering=last_updated) is `adisakshya/event-service` and it has the following tags -
- 1.0.0-dev
- 1.0.0-prod

The production environment uses the docker-image tagged with `prod` and the development environment uses the docker-image tagged with `dev` i.e., in case of event-service `adisakshya/event-service:1.0.0-prod` will be the docker-image used for deploying the event-service to production environment and `adisakshya/event-service:1.0.0-dev` will be used to deploy it to the development environment.
