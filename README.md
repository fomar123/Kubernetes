# Demo:ConfigMap & Secret Volume
##### Create Mosquitto Deployment Without Volumes:
- Create a Mosquitto Deployment in a YAML file (mosquitto-without-volumes.yaml)
- Define metadata, spec, and containers with the Mosquitto image and ports

##### Create ConfigMap Component to Overwrite mosquitto.conf File:
- Define a ConfigMap in a YAML file (configmap-file.yaml).
- Include metadata and data for mosquitto.conf customization.
- Apply the ConfigMap using:
  
         kubectl apply -f configmap-file.yaml.
  
- Create Secret Component for Passwords File:

##### Define a Secret in a YAML file (secret-file.yaml).
- Include metadata and base64-encoded data for usernames and passwords.
- Apply the Secret using:
  
         kubectl apply -f secret-file.yaml.
  
- Adjust Mosquitto Deployment to Include Volumes:

##### Edit the mosquitto.yaml file.
- Add volumeMounts to the Mosquitto container for mounting ConfigMap and Secret.
- Add volumes to the Deployment spec, referencing the ConfigMap and Secret.
- Apply the updated Deployment using:

   kubectl apply -f mosquitto-deployment.yaml.
  
##### Verify Changes in Mosquitto Pod:
- Use kubectl get pods to check the status of the Mosquitto Pod.
- Access the Pod's shell using:

        kubectl exec -it <pod-name> -- /bin/sh.
- Inside the Pod, verify the mosquitto.conf and passwords file in the mounted volumes.
- Exit the Pod shell using the exit command.

# Project: Install a Stateful Application on K8s using Helm
##### Created K8s cluster on Linode Kubernetes Engine:
##### •	Kubeconfig is necessary to access the cluster from the local machine 
##### •	Download kubeconfig.yaml from linode and set it as an environmental variable:

       	export KUBECONFIG=test-kubeconfig.yaml
##### Deploy replicated MongoDB (StatefulSet using Helm Chart) and configured Data Persistence with Linode Block Storage:
• add helm chart  repository:

      helm repo add bitnami https://charts.bitnami.com/bitnami
• create a yaml file to overwrite the parameters
• to overwrite execute command:

          helm install [our name] –-values [value file name] ]chart name]

##### Deployed replicated MongoDB (StatefulSet using Helm Chart) and configured Data Persistence with Linode Block Storage: 
• add helm chart  repository:

       helm repo add bitnami https://charts.bitnami.com/bitnami
       
• create a yaml file to overwrite the parameters

• to overwrite execute command

        helm install [our name] –-values [value file name] ]chart name]

##### Deployed MongoExpress (Deployment and Service):
• create a mongo-express deployment configuration 

• execute command to deploy mongo-express:

          kubectl apply -f test-mongo-express.yaml

##### Deployed NGINX Ingress Controller as Loadbalancer (using Helm Chart:
• add helm repository: 

     helm repo add stable https://charts.helm.sh/stable
• install helm chart: 

      helm install nginx-ingress stable/nginx-ingress --set controller.publishService.enabled=true

##### Configured Ingress rule and execute command:  

       kubectl apply -f test-ingress.yaml

To scale down stateful set:

      kubectl scale –replicas=0 statefulset/mongodb



# Project: Deploy App from Private Docker Registry:
##### Log in to AWS Container Repository (ECR):
- Use the docker login command to authenticate your Docker client with your AWS ECR registry:

       docker login -u AWS -p <AWS_ACCESS_KEY_ID> -e none <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com
  
- Replace <AWS_ACCESS_KEY_ID>, <AWS_ACCOUNT_ID>, and <AWS_REGION> with your AWS credentials.

##### Create Docker Config JSON File:
- Create a config.json file with Docker credentials. You can generate it using a script like:

         echo "{\"auths\":{\"<AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com\":{\"username\":\"AWS\",\"password\":\"<AWS_ACCESS_KEY_ID>\"}}}" > config.json
- Then, base64 encode the config.json file:

       cat config.json | base64
##### Create Kubernetes Secret:
- Create a Kubernetes Secret using the base64-encoded config.json value and apply it
- Apply the Secret using

         kubectl apply -f ecr-secret.yaml.

##### Configure Deployment for Demo App:
- Define a Deployment configuration YAML for your demo app (my-app-deployment.yaml).
  
- In the Deployment spec, add the Secret reference under imagePullSecret
  
![Screenshot 2023-08-13 at 02 19 51](https://github.com/fomar123/Kubernetes/assets/90075757/2dac5782-7e98-4567-96fe-df738a75a8bc)

- Apply the Deployment using:

        kubectl apply -f demo-app-deployment.yaml.

  
# Deploy Prometheus Stack using Helm
##### Install Prometheus Operator Helm Chart:
- Use Helm to install the Prometheus Operator Helm chart:

       helm install [RELEASE_NAME] prometheus-community/kube-prometheus-stack
  
##### Access Grafana UI (Port Forwarding):
- Forward local port to Grafana deployment:

      kubectl port-forward deployment/prometheus-grafana 3000

##### Access Prometheus UI (Port Forwarding):
- Forward local port to Prometheus pod:

       kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0 9090



# Project: Deploy Microservices Application
##### Created YAML file with 11 Deployments and Services:
- Created a  YAML file (config.yaml) containing configurations for 11 Deployments and Services
  
##### Kubernetes Cluster Setup:
- Set up a Kubernetes cluster on Linode with 3 worker nodes.

##### Cluster Connection:
- Connected to the Kubernetes cluster using the kubeconfig.yaml file:
  
        export KUBECONFIG=~/Downloads/online-shop-microservices-kubeconfig.yaml
  
##### Namespace and Deployment:
- Created a dedicated namespace for microservices:
  
            kubectl create ns microservices

- Deployed all microservices using the config.yaml file within the microservices namespace:

                kubectl apply -f config.yaml -n microservices

##### Accessing Application:
- Accessed the Online Shop application through a web browser after deploying the microservices and services

   
# Improved Microservices Config Files: Security Best Practices:

##### Version Tagging:
-Added version tags to each container image for better image management and traceability.

##### Liveness Probe:
- Configured a Liveness Probe for each container to ensure their health:

            livenessProbe:
  
        periodSeconds: 5
  
          exec:
  
        command: ["/bin/grpc_health_probe", "-addr=:8080"]

  
##### Readiness Probe:
- Configured a Readiness Probe for each container to indicate their readiness:

           readinessProbe:
  
      periodSeconds: 5
  
      exec:
  
      command: ["/bin/grpc_health_probe", "-addr=:8080"]

##### Resource Requests and Limits:
- Set resource requests and limits for CPU and memory usage for each container:  

      resources:
        requests:
          cpu: 100m
          memory: 64Mi
        limits:
         cpu: 200m
         memory: 128Mi

##### Replica Configuration:
- Configured more than one replica for each Deployment to enhance availability and redundancy

# Project: Create Helm Chart for Microservices and Deploy with Helmfile:

##### Helm Chart Creation:
- Developed a Helm chart named "microservices" for the Microservices Application
- Prepared individual values.yaml files for each microservice within the Helm chart
  
##### Redis Helm Chart:
- Created a separate Helm chart for deploying Redis
  
##### Deploying Microservices with Helm:
-Deployed the Microservices Application using Helm with custom values:

           helm install -f [values file] [release name] [chart name]

##### Helmfile Setup:
- Established a Helmfile to manage multiple Helm releases.
  
#### Install Helmfile:
- Synchronized and deployed Helm releases using Helmfile:

             helmfile sync
  
# Project: Deploy to EKS Cluster from Jenkins Pipeline:
##### Jenkins Container Setup:
- Installed kubectl inside the Jenkins container using:

         curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.1/2023-04-19/bin/linux/amd64/kubectl

- Installed aws-iam-authenticator inside the Jenkins container using:

       curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/aws-iam-authenticator_0.5.9_linux_amd64
       chmod +x ./aws-iam-authenticator
       mv ./aws-iam-authenticator /usr/local/bin
       
##### Jenkins Container Configuration:
- Created a config file for Kubernetes and copied it into the Jenkins container:

       docker cp config f16b145b5924:/var/jenkins_home/.kube

##### Jenkins Configuration:
- Set up a Jenkins credential to securely store necessary credentials.

  
##### Jenkinsfile Creation:
- Created a Jenkinsfile that defines the steps to deploy an EKS cluster using the configured tools and credentials.         
 

# Project: Create EKS Cluster with Node Group:

##### EKS Cluster Setup:
- Created an EKS Role to grant necessary permissions for the EKS cluster
- Created a Virtual Private Cloud (VPC) using a CloudFormation template
- Deployed an EKS cluster using the AWS Management Console or CLI
  
##### Local Kubectl Configuration:
- Created a kubeconfig file for local kubectl access:

          aws eks update-kubeconfig --name [cluster name]
  
##### Node Group Creation:
- Created a Node Group Role to define permissions for worker nodes
- Set up an EC2-based Node Group to provide worker nodes for the EKS cluster
 
##### Auto-Scaling Configuration:
- Configured auto-scaling for the EKS cluster by deploying the cluster-autoscaler Pod:

             kubectl apply -f https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler- autodiscover.yaml

- Created a new policy for auto-scaling permissions and attached it to the Node Group Role
  
##### Deploy Nginx Pod and Service:
- Deployed an Nginx Pod and Service in the EKS cluster using a nginx-config.yaml file.



            
           
#    Project - Create Pipeline and deploy to Linode Kubernetes cluster:
#####      Created LKE cluster and Download kubeconfig.yaml from linode and set it as an environmental variable:
                export KUBECONFIG=~/Downloads/LKS-kubeconfig.yaml
#####        Created Jenkins Credential with kubeconfig file
##### Installed Kubernetes CLI Plugin on Jenkins
##### Created Jenkinsfile that deploys to LKE cluster
 
 
 
# Project - Complete CI/CD Pipeline with DockerHub:
#####  Created Deployment and Service for App deployment
#####  Adjust Jenkinsfile to set environment variables with envsubs
##### Installed “gettext-base” tool inside Jenkins Container on DigitalOcean Server in CLI:
       First go to root user :
             docker exec -it -u 0 <container name> bash
	   Install gettext-base:
              apt-get install gettext-base 
##### Created Secret for DockerHub Registry in EKS cluster (connect to EKS cluster if not already) and reference it in the  Deployment file:
        kubectl create secret docker-registry my-registry-key \
        --docker-server=docker.io \
        --docker-username=fomar123 \
        --docker-password=<your docker passoword>
#####  Executed Jenkins Pipeline 


#  Project - Complete CI/CD Pipeline with AWS ECR
##### Created ECR Repository in AWS
##### Created Credential for ECR repository in Jenkins
##### Created Secret for AWS ECR Registry in EKS cluster and adjusted reference in Deployment file: 

	   Create Secret in CLI:
               kubectl create secret docker-registry aws-registry key --docker-server=<your docker repository url> \
               --docker-username=<your username> \
               --docker-password=<your password >

 
