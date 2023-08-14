# kamma-k8-manifest-files

# Note before starting 

Please ensure that the minikube k8s cluster is up and running before attempting to deploy the k8s manifest files.

If the pipline has run successfull the kamma web application has been automitically deplpyed to the Kubernetes cluster running in the EC2 instance. 

All we need to do is view the service from a web browser now ...

# Instructions


(1) `minikube tunnel`

- Open a new SSH terminal and login again to the EC2 instance. This command creates a network route on the host to the service using the cluster's IP address.

(2) `kubectl get svc`

- If we run `curl http://<external-ip>:8080` on the application in a new SSH windows we see that the service has an external IP address but this is only accessible internally within the EC2 instance. 


(3) `kubectl port-forward svc/kamma-web-app 8080:8080`

- After running step (2) in that SSH terminal we need to port forward to create a tunnel between the ec2 instance (locally) and the service running in minikube. This allows the application to run as though it were running locally on the ec2 instance. If we were to curl `http://127.0.0.1:8080` inside the EC2 instance the application would also repsond.  

(4) `ssh -i "<key-pem>" ec2-user@<dns-address> -L 0.0.0.0:8080:127.0.01:8080`

- We now need to open a third SSH terminal which will allow us to connect to the port 8080 on the ec2 instance from our local workstation.

(5) Finally open a browser and hit http://127.0.0.1:8080. You should now see the application running locally from our local workstation. 
