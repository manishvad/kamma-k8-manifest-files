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

(7) `minikube service kamma-web-app`

- Open the  service
