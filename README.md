# Demo:ConfigMap & Secret Volume
##### Created Mosquitto Deployment without any volumes
##### Created ConfigMap component to overwrite mosquitto.conf file.
##### Created Secret component to add passwords file
##### Created Secret component to add passwords file
##### Adjusted Mosquitto Deployment to include volumes



# Project: Install a Stateful Application on K8s using Helm
##### Created K8s cluster on Linode Kubernetes Engine:
##### •	Kubeconfig is necessary to access the cluster from the local machine 
##### •	Download kubeconfig.yaml from linode and set it as an environmental variable:
##### •	export KUBECONFIG=test-kubeconfig.yaml
##### • Deployed replicated MongoDB (StatefulSet using Helm Chart) and configured Data Persistence with Linode Block Storage:
##### •	add helm chart  repository: helm repo add bitnami https://charts.bitnami.com/bitnami
##### •	create a yaml file to overwrite the parameters
##### •	to overwrite execute command : helm install [our name] –-values [value file name] ]chart name]

Deployed replicated MongoDB (StatefulSet using Helm Chart) and configured Data Persistence with Linode Block Storage: 
##### •	add helm chart  repository: helm repo add bitnami https://charts.bitnami.com/bitnami
##### •	create a yaml file to overwrite the parameters
##### •	to overwrite execute command : helm install [our name] –-values [value file name] ]chart name]

Deployed MongoExpress (Deployment and Service):
##### •	create a mongo-express deployment configuration 
##### •	execute command to deploy mongo-express : kubectl apply -f test-mongo-express.yaml

Deployed NGINX Ingress Controller as Loadbalancer (using Helm Chart:
##### •	add helm repository:  helm repo add stable https://charts.helm.sh/stable
##### •	install helm chart: helm install nginx-ingress stable/nginx-ingress --set controller.publishService.enabled=true

Configured Ingress rule and execute command:  kubectl apply -f test-ingress.yaml

To scale down stateful set: kubectl scale –replicas=0 statefulset/mongodb

