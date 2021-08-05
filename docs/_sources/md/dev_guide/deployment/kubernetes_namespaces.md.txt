# Kubernetes Namespaces

> Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces and provide logical separation between the teams and their environments.

At the deployment level, i.e., on Kubernetes the deployment environments are distinguished using namespaces.
Both development and production environments have their own namespaces, which logically separates them and allows modification in one without affecting the other environment. Using namespaces as environments makes it easier to manage workload for a project from a single Kubernetes cluster and this avoids the pain of maintaining multiple Kubernetes clusters for multiple environments.

![Kubernetes Dashboard showing production and development namespaces](https://github.com/adisakshya/continuous-improvement/blob/master/assets/utils/environments.png?raw=true)

The `production` namespace wraps the production workload i.e., the source-code corresponding to the production-version/branch of each microservice is deployed in this namespace. The `development` namespace wraps the development workload i.e., the source-code corresponding to the development-version/branch of each microservice is deployed in this namespace.

Each environment has a unique identifier prefix asssigned to it, for production environment it is `prod` and for development environment it is `dev`. The unique prefix is used while naming the deployments, replica-sets, pods and services in the environment.
For example the [Notification Service](https://github.com/adisakshya/notification-service) is named as `prod-notification-service` and `dev-notification-service` when deployed to production environmentand development environment respectively. These prefix are also used while tagging the [docker-images](https://hub.docker.com/r/adisakshya/notification-service/tags?page=1&ordering=last_updated) for the microservices to separate production and development packages.

Each environment has it own set of configurations, deployments and services running and independently interacting with various external services like AWS SNS, SQS, RDS, Firebase Cloud Messaging without interferring with the other environment.

The namespaces are created using the following YAML configuration -

```yml
apiVersion: v1
kind: Namespace
metadata:
  name: development
  labels:
    name: development

---

apiVersion: v1
kind: Namespace
metadata:
  name: production
  labels:
    name: production
```
