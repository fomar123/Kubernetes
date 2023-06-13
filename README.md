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

       	export KUBECONFIG=test-kubeconfig.yaml
##### • Deploy replicated MongoDB (StatefulSet using Helm Chart) and configured Data Persistence with Linode Block Storage:
##### •	add helm chart  repository: helm repo add bitnami https://charts.bitnami.com/bitnami
##### •	create a yaml file to overwrite the parameters
##### •	to overwrite execute command:
          helm install [our name] –-values [value file name] ]chart name]

Deployed replicated MongoDB (StatefulSet using Helm Chart) and configured Data Persistence with Linode Block Storage: 
##### •	add helm chart  repository: helm repo add bitnami https://charts.bitnami.com/bitnami
##### •	create a yaml file to overwrite the parameters
##### •	to overwrite execute command : helm install [our name] –-values [value file name] ]chart name]

Deployed MongoExpress (Deployment and Service):
##### •	create a mongo-express deployment configuration 
##### •	execute command to deploy mongo-express: 
          kubectl apply -f test-mongo-express.yaml

Deployed NGINX Ingress Controller as Loadbalancer (using Helm Chart:
##### •	add helm repository:  helm repo add stable https://charts.helm.sh/stable
##### •	install helm chart: helm install nginx-ingress stable/nginx-ingress --set controller.publishService.enabled=true

Configured Ingress rule and execute command:  kubectl apply -f test-ingress.yaml

To scale down stateful set: kubectl scale –replicas=0 statefulset/mongodb



# Project: Deploy App from Private Docker Registry:
##### Logg in to AWS Container Repository | docker login and create docker config.json file
##### Create Secret component
##### Configure Deployment for demo app

# Deploy Prometheus Stack using Helm
##### Install Prometheus Operator Helm Chart:
 helm install [RELEASE_NAME] prometheus-community/kube-prometheus-stack

##### Accessed Grafana UI (configured port-forward): 
kubectl port-forward deployment/prometheus-grafana 3000


##### Accessed Prometheus UI (configured port-forward):
kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0 9090

# Project: Deploy Microservices Application
##### Created YAML file with 11 Deployments and Services
##### Created a K8s cluster with 3 Worker Nodes on Linode
##### Connect to Kubernetes Cluster With Kubeconfig.yaml file:
export KUBECONFIG=~/Downloads/online-shop-microservices-kubeconfig.yaml 
##### Created a Namespace and deployed all the microervices into it:
kubectl create ns microservices
kubectl apply -f config.yaml -n microservices 
##### Accessed Online Shop with Browser 

# Project: Improved Microservices Config Files. Security Best Practices
##### Added version to each container image
##### Configured Liveness Probe for each containe:

 
    livenessProbe:
   
    periodSeconds: 5
          
          exec:
          
            command: ["/bin/grpc_health_probe", "-addr=:8080"]
##### Configured Readiness Probe for each container:

  
 
      readinessProbe:
  
       periodSeconds: 5
          
          exec:
          
            command: ["/bin/grpc_health_probe", "-addr=:8080"]

 ##### Configured Resource Requests resources and Resource Limits: 
 
          requests:
          
            cpu: 100m
            
            memory: 64Mi
            
          limits:
          
            cpu: 200m
            
            memory: 128Mi
        
 #####  Configure more than 1 Replica for each Deployment     
  
 # Project: Create Helm Chart for Microservices and Deploy Microservices with Helmfile
 
 #####  Created “microservices” Helm Chart
 
 #####  Created values.yaml files for each microservice
 
 #####  Created “redis” Helm Chart
 
 #####  Deployed Microservices Application with “helm install”:
    
    helm install -f [name of file] [realse name] [chart name]
    
 ##### Created Helmfile
 ##### Install Helmfile:
         helmfile sync
         
 # Project: Deploy to EKS cluster from Jenkins Pipeline
 
##### Installed kubectl inside Jenkins Container:
      curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.1/2023-04-19/bin/linux/amd64/kubectl      

##### Installed aws-iam-authenticator inside Jenkins Container:
        curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/aws-iam-authenticator_0.5.9_linux_amd64

##### 
        Apply execute permissions to the binary:
           chmod +x ./aws-iam-authenticator
           
        Move aws authenticator file: 
           mv ./aws-iam-authenticator /usr/local/bin


##### Created ./kube/config and copied inside the Jenkins Container:
          docker cp config f16b145b5924:/var/jenkins_home/.kube
          
##### Created Jenkins Credential  

##### Created Jenkinsfile that deploys an EKS cluster

      
#  Project: Create EKS cluster with Node Group
 
##### Created EKS Role 
##### Created VPC with Cloudformation Template
##### Created EKS cluster
##### Connected to EKS cluster with kubectl locally:
        create kubeconfig file: 
           aws eks  update-kubeconfig  --name  [name of cluster]
 ##### Created Node Group Role 
 ##### Created Node Group: EC2 Instances - Worker Nodes
 #####  Configure Auto-Scaling - Deployed cluster-autoscaler Pod
          Created new Policy for Auto-Scaling Permission
          Attached new Policy to existing Node Group Role
          Deployed Autoscaler Component in EKS cluster:
             kubectl apply -f https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml 
        
 #####  Deployed Nginx Pod and Service:  
           kubectl apply -f nginx-config.yaml
           
 
 #     Project - Create Pipeline that deploys to EKS cluster
 
 #####  Installed kubectl inside Jenkins Container: 
           curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.1/2023-04-19/bin/linux/amd64/kubectl
 #####  Installed aws-iam-authenticator inside Jenkins Container:
            curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/aws-iam-authenticator_0.5.9_linux_amd64
 #####  Apply execute permissions to the binary:
                  chmod +x ./aws-iam-authenticator
 #####  Move aws authenticator file:
          mv ./aws-iam-authenticator /usr/local/bin
 
