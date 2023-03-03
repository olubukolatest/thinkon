# Thinkon
Take-home Task

This is a Kubernetes YAML manifest file. It defines several Kubernetes objects that will be deployed in the production namespace.

The file includes the following Kubernetes objects:

Namespace: Defines the production and staging  environment. This can be changed on the .yml file for each environment.

ConfigMap: Defines a ConfigMap named mongo-configmap that stores some configuration information (i.e. database username, database hostname, and database password). This ConfigMap will be used by the app deployment to connect to the MongoDB instance.

Secret: Defines a Secret named mongodb-password that stores the MongoDB password in an encoded format. This Secret will be used by the app deployment to connect to the MongoDB instance.

Deployment: Defines a Deployment named userapp and shiftsapp that deploys replicas of a container. This Deployment will use the ConfigMap and Secret defined above to connect to the MongoDB instance.

Service: Defines a Service named springapp that exposes the app Deployment on port 80.

PersistentVolumeClaim: Defines a PersistentVolumeClaim named mongodbpvc that requests 16GB of storage.

StatefulSet: Defines a StatefulSet named db that deploys three replicas of a MongoDB container. This StatefulSet will use the ConfigMap and Secret defined above to configure the MongoDB instance.

Service: Defines a Service named mongo that exposes the db StatefulSet on port 27017.

Role: Defines a Role named deployment that grants permission to create, update, delete, get, list, watch, and rollback Deployments, and get, list, and watch Pods and Services in the production namespace.

RoleBinding: Defines a RoleBinding named deployment-binding that binds the deployment Role to a specific user (specified by their username) in the production namespace.


The HPA will ensure that the number of replicas is automatically scaled up or down to maintain the target CPU utilization percentage of 70%. I also included the necessary resources limits and requests for the container, as well as a maxUnavailable of 25% and a maxSurge of 1 in the RollingUpdate strategy.

To ensure that the deployment can handle rolling deployments and rollbacks, i used the RollingUpdate strategy in the deployment. This strategy will update the pods in a rolling manner, one by one, ensuring that there is no downtime for the application. Additionally, it allows for rollbacks to previous versions in case of any issues with the new version.

Regarding IAM controls for the development team, a YAML configuration is included where i used Kubernetes Role-Based Access Control (RBAC) to grant the development team the necessary permissions to deploy and rollback but restrict their access to certain commands in the Kubernetes cluster.
