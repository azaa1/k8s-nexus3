# Setting Up Sonatype Nexus 3 on Kubernetes Cluster.

### This Repository Sets Up  Sonatype Nexus 3 on your existing Kubernetes Cluster.

* The folder nexus contains the following:

### namespace.yml 

##### Run :  kubectl create -f namespace.yml 

* This will create a namespace called 'nexus' on your cluster. 


### nexus-pv.yml 

##### Run : kubectl create -f nexus-pv.yml 

* This will create a 'persistent volume' called "nexus-volume" , attach a label "app: nexus" to it. The size of the volume is 10Gi. And the path to the volume is "/tmp/volume-local". 

#### NOTE: change the 'size' and 'path' based on your needs. Setting the 'path' to a folder under 'tmp' is not recommended. 


### nexus-pvc.yml 

##### Run : kubectl create -f nexus-pvc.yml 

* This will create a 'persistent volume claim' called 'nexus-pvc' and a resource request of 10Gi. 


### nexus-deployment.yml 

##### Run : kubectl create -f nexus-deployment.yml 

* This will create a 'deployment' called 'nexus-deployment' under namespace 'nexus'. It sets the replica to 1, attaches the label 'app: nexus' to deployment. And does the following: 
  - Dowloads the container image 
  - Attaches the volume to the container
  - Sets env inside the container 
        * This step is explained and found in the Docker Hub under Nexus image
  - Sets the resources request for the container
  - Exposes port 8081 of the container (Nexus uses port 8081)
  - Mounts the volume to a path inside the container
  
  
  ### nexus-service.yml
  
  ##### Run : kubectl create -f nexus-service.yml 
  
  * This will create a 'service' called 'nexus-service'. The type of the service is 'LoadBalancer' which publishes the port '31000' of the node through port 8080 of the service, sets the protocol to TCP and sets the target port to '8081' (this is the port of our container). 
  
  #### NOTE : service type 'LoadBalancer' works only on cloud providers that provider Load-Balancer service.
  
  
  #### If everything went well. You can run the following to login to Nexus: 
  
  ##### Run : kubectl -n nexus get services
  
  * The output should show a service called 'nexus-service'
  * Copy the EXTERNAL-IP of your service, and paste on your browsers >  IP:8080
  * You should see the login page for Nexus. 
  * Default Login to Nexus:
    - username = admin
    - password = admin123
