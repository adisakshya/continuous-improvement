
# Configurations and Environment Variables

There were a lot of reasons to use environment variables in this type of project the most important one's were to enable easier switching between multiple deployment environments and including configurations for [external services]().

There are multiple ways to define environment variables with Kubernetes -

**Using configuration file**

Environment variables can be included using the `env` field in the YAML configuration file. Environment variables that are set following this way override any environment variables specified in the container image.

**Using secrets**

It allows us to inject multiple values at once from a file. They’re more suited to sensitive data like passwords, API keys, etc.  For example, database passwords, private SSH keys, and certificates should all go into secrets. Reminder and Notification microservices uses Amazon RDS PostgreSQL Database and thus require appropriate credentials to use the database, the Notification microservice also requires Fiebase Cloud Messaging Configuration JSON to be able to function.

Following is an example of YAML configuration that can be used to set secrets -

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: prod-ses-secrets
  namespace: production
  resourceVersion: "1"
type: Opaque
data:
  # Name of reminder-email-template
  reminder_template: UHJvZHVjdGlvblJlbWluZGVyVGVtcGxhdGU=

---

apiVersion: v1
kind: Secret
metadata:
  name: dev-ses-secrets
  namespace: development
  resourceVersion: "1"
type: Opaque
data:
  # Name of reminder-email-template
  reminder_template: RGV2ZWxvcG1lbnRSZW1pbmRlclRlbXBsYXRl
```

Following is an example of YAML configuration that is used to create a deployment while utilizing secrets. Here `env` is the name of deployment environment, `env_prefix` is either 'dev' or 'prod', `resource_name` is the name of microservice for example - 'event-service', `docker_image_name` and `docker_image_tag` represent the name and tag of the docker image associated with the `resource_name`.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: '{{ env_prefix }}-{{ resource_name }}'
  namespace: '{{ env }}'
  labels:
    app: '{{ env_prefix }}-{{ resource_name }}'
spec:
  selector:
    matchLabels:
      app: '{{ env_prefix }}-{{ resource_name }}'
  replicas: 1
  template:
    metadata:
      labels:
        app: '{{ env_prefix }}-{{ resource_name }}'
    spec:
      containers:
        - name: '{{ env_prefix }}-{{ resource_name }}'
          image: '{{ docker_image_name }}:{{ docker_image_tag }}'
          imagePullPolicy: Always
          ports:
            - containerPort: 80
          env:
            - name: NODE_ENV
              value: '{{ env }}'
            - name: DB_HOST
              valueFrom:
                secretKeyRef:
                  name: '{{ env_prefix }}-reminder-db-secrets'
                  key: host
            - name: DB_NAME
              valueFrom:
                secretKeyRef:
                  name: '{{ env_prefix }}-reminder-db-secrets'
                  key: name
            - name: DB_PASS
              valueFrom:
                secretKeyRef:
                  name: '{{ env_prefix }}-reminder-db-secrets'
                  key: password
            - name: DB_USER
              valueFrom:
                secretKeyRef:
                  name: '{{ env_prefix }}-reminder-db-secrets'
                  key: user
            - name: EVENT_TOPIC_ARN
              valueFrom:
                secretKeyRef:
                  name: '{{ env_prefix }}-sns-topics-secrets'
                  key: event_topic
```
