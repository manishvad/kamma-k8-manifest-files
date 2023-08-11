# kamma-k8-manifest-files

# Note before starting 

Please ensure that the minikube k8s cluster is up and running before attempting to deploy the k8s manifest files



# Instructions

Please follow step by step ...

(1) `git clone https://github.com/manishvad/kamma-k8-manifest-files.git`

- Clone this repository to the EC2 Instance in `/home/ec2-user`

(2) `aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin <aws_account_number>.dkr.ecr.eu-west-1.amazonaws.com`

- Run this command on the EC2 instance to retrieve an authentication token and authenticate your Docker client to your registry.

(3) `docker pull <aws_account_number>.dkr.ecr.eu-west-1.amazonaws.com/kamma-ecr-repo:kamma-whoami-<tag_number>`

- Pull latest docker image from ECR with latest tag (vist ECR to get the latest tag)

(4) `minikube image load <aws_account_number>.dkr.ecr.eu-west-1.amazonaws.com/kamma-ecr-repo:kamma-whoami-<tag_number>`

- Load latest docker image into minikube images.

(5) `minikube addons enable ingress`

- Enable the minikube ingress controller

(6) `kubectl apply -f deployment.yaml` && `kubectl apply -f service.yaml`

- Apply the k8s manifest files & the web application will be deployed.

(8) `minikube tunnel`

- Open a new SSH terminal and login again to the EC2 instance. This command creates a network route on the host to the service using the cluster's IP address.

(9) `kubectl get svc`

- In the previous SSH terminal we already have running will see that the service has an external IP address but this is only accessible internally within the EC2 instance. If we run `curl http://<external-ip>:8080` the application will respond successfully.

(10) `kubectl port-forward svc/kamma-web-app 8080:8080`

- After running step (9) in that SSH terminal we need to port forward to create a tunnel between the ec2 instance (locally) and the service running in minikube. This allows the application to run as though it were running locally on the ec2 instance. If we were to curl `http://127.0.0.1:8080` inside the EC2 instance the application would also repsond.  

(11) `ssh -i "<key-pem>" ec2-user@<dns-address>` -L 0.0.0.0:8080:127.0.01:8080`

- We now need to open a third SSH terminal which will allow us to connect to the port 8080 on the ec2 instance from our local workstation.

(12) Finally open a browser and hit http://127.0.0.1:8080. You should now see the application running locally from our local workstation. 
