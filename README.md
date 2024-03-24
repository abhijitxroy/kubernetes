# kubernetes

Link: Kubernetes Tutorial for Beginners | What is Kubernetes? Architecture Simplified!



Architecture: 



Why do we use Docker?
	- 
Configuration management
	- Immutability => For running conatiner you can not mutate the configiration
	- Reliability -> due to above 
	- cheff, puppet => infrastructure as code principle => basically writing configuration about your server
	
Managing containers





Package maneger issue will be resolved by the below structure.
Bare metal server => Virtual machine => Containerization
monolithic => Microservices

NOTES: A bare metal server is a physical server dedicated entirely to a single user
 
Service mesh : A service mesh is a configurable infrastructure layer that allows communication between microservice applications. 
			It's a dedicated infrastructure layer that controls service-to-service communication over a network.

One application on one VM, which is not good for scaling, Hence Docker come into the picture.

Scenario-1: You have 100 container and people start using your website and no is increasing, so server load increases => So you need to have more no of instances of your application
Scenario-2: you have 1000 container and out of 1000, 200 got damaged. So, you need to restart those => which is another problem.
Scenario-3: you have v1 running currently but you want to update v2, for this another problem to update dynamically otherwise need to stop and 
			update which is another problem for rollback with zero downtime also deploy your application 
To resolve this, Orchestator comes into picture.

Orchestrators:
	- Helps us in deploying and managing application dynamically
		○ Scaling: More user base scale it up
		○ Heal: Self Heal the container when some container going down => restart those container
		○ Zero downtime : roolback with zero downtime 
		○ Replicate any services/container
		
NOTES: These are all fetaures of Cloud-native application to meet the mordern busieness demands. Kubernates also part of this.
For example, The master of orchestrator is the kubernates and the players(violin, trumpet, guiter, drum) are microservices.
	
Introduction to Kubernetes
	- Kubernates is much more than a container orchestrator
	- Load balancing
	- Fault tolerance
	- Service discovery
	- Volume => add external disk
	- Secrets
	- etc
History of Kubernetes
	- AWS offeres cloud 
	- Openstack came to beat them but lost
	- Google internally using billions of  orchestator containers already privately
		○ Borg
		○ Omega
	- Then Google created kubernates from BORG/OMEGA 
	- Google made Kubernates(K8S) open source in 2014 and donated it to CNCF(Cloud Native Computing Foundation)
	
Docker vs Kubernetes
	- Docker is container tool
	- Kubernates acts as orchestrator and much more
	- Earlier Docker was used in K8s but now it got deprecated due to CRI(container runtime Interface)
	- bcz K8s is allowing you to use various Container runtime interface like containerD. 
	- It comply with the standard of Open container initiative(OCI)
	
Terminologies
Cluster 
		○ k8s cluster = Control plane +  nodes
		○ Control plane previously know as Master nodes
		○ Nodes can be treated as a VM 
	
		○ Worker Node: is the place where you apllication will be running
		○ Control plane : is going to manage the worker nodes
KubeCTL:
		○ KubeCTL is the kunbernates command line tool
		○ Kubectl controls the Kubernetes Cluster. It is one of the key components of Kubernetes which runs on the workstation on any machine when the setup is done.
		○ KubeCTL will communicate with control plane.
			§ Interact with 2 ways 
				□ Declarative way
					® Create manifest file, write yaml file 
				□ Imperative way 
					® Provide specific command on terminal directly
Ingres: 
		○ A Kubernetes ingress is an API object used to manage external user access to services running in a Kubernetes cluster. 
			§ It provides routing rules, defined within the ingress resource, which you can use to configure access to your clusters. The HTTPS/HTTP protocol is commonly used to facilitate routing.
		○ Ingress and egress - From the point of view of a Kubernetes pod, ingress is incoming traffic to the pod, and egress is outgoing traffic from the pod.
		
Pod
		○ Pod is nothing but a Scheduling unit in Kubernates
		○ Where it resides:
			§ Worker Node => Container runtimr => Pod
		
		○ similar to container it's a scheduling unit, one more layer added
		○ simplest smallest scheduling unit in kubernates is called POD
		○ Good practise: One container one pod  or one type of service

Step for running application in K8s
	- Create microservices
	- Containerize it - Add every microservice in its container 
	- Put every container in pods
	- Deploy these pods to controllers in Kubernates such as deployment controller(In Kubernates, deployment controller is the built-in controller) 
		○ To understand better you can image pods are premitive data and Deployments are Array 
	
Control Plane
		○ Control plane helps us in managing the overall health of the cluster. Like
			§ you want to create a new pod
			§ you want to scale pods
			§ you want to destroy something 
			§ you want to espose something
			§ you want to do anything it will be managed via the control plane
		○ Architecture 
			§ Various components
				□ Kube-Api Server 
					® The core of Kubernetes control plane is the Kube-api-server
					® The Kube API server is the entry point for all the REST commands used to control the cluster.
					® The main purpose of the API server in Kubernetes is to receive and process API calls in the form of HTTP requests.
			
				□ Kube-Control-manager 
					® The Kubernetes controller manager, also known as kube-controller-manager, is a daemon that acts as a continuous control loop in a Kubernetes cluster. 
					It's a component of the control plane that runs in the form of a container within a Pod, on every master node.
					® Controller manger listens to the API Server. It has four functionalites. 
						◊ Desired state :  check the desired state. like, api server said 5 pods run always
						◊ Current state: Monitoring the current state of your cluster through the API Server
						◊ Differences: It will constantly listening to the API server, like someone is requesting to changes in deployement
						◊ Make the changes: Making appropriate changes to keep the application running by ensuring sufficient Pods are in a healthy state
						◊ Noticing and responding when nodes go down 
				□ ETCD (DB) - store the information about the cluster, API Server will get the data from ETCD
				□ Kube-Scheduler - Kube-scheduler selects an optimal node to run newly created or not yet scheduled (unscheduled) pods. 
					® kube-scheduler selects a node for the pod in a 2-step operation:
						1. Filtering - The filtering step finds the set of Nodes where it's feasible to schedule the Pod. 
						2. Scoring - In the scoring step, the scheduler ranks the remaining nodes to choose the most suitable Pod placement. 
						The scheduler assigns a score to each Node that survived filtering, basing this score on the active scoring rules.
					® Finally, kube-scheduler assigns the Pod to the Node with the highest ranking. If there is more than one node with equal scores, kube-scheduler selects one of these at random.
					
				


Architecture of Kubernetes
Diagram


	- Kubectl 
	- Control plane
	- Worker node
		○ kubelet 
			§ The kubelet is the primary "node agent" that runs on each node.
			§ It can register the node with the apiserver using one of: the hostname; a flag to override the hostname; or specific logic for a cloud provider.
			§ The kubelet works in terms of a PodSpec. A PodSpec is a YAML or JSON object that describes a pod. The kubelet takes a set of PodSpecs that are provided through various mechanisms (primarily through the apiserver) and ensures that the containers described in those PodSpecs are running and healthy. The kubelet doesn't manage containers which were not created by Kubernetes.
			Other than from a PodSpec from the apiserver, there are two ways that a container manifest can be provided to the kubelet.
				• File: Path passed as a flag on the command line. Files under this path will be monitored periodically for updates. The monitoring period is 20s by default and is configurable via a flag.
				• HTTP endpoint: HTTP endpoint passed as a parameter on the command line. This endpoint is checked every 20 seconds (also configurable with a flag).
		○ Container runtime Interface
			§ It basically Docker thig but right now it replaced with dockerD as Docker does not support CRI.
		○ Kube proxy
			§ It's repsonsible for networking, If worker node is trying to communicate with outside network kube-proxy will help in that.
			§ kube-proxy will provide IP address to Node.
		
Deployment:
	- Pod is the smallest scheduling unit and pod is schedule inside a deployment
	- Deployements will scale your pods and update your pods etc.
	
	
Kubernetes DNS
	- For interpod cluster communication, K8s has internal DNS service
	- 
	- Whenever you creates a new service it automatically get register into that.

Installation
	- kubctl --- click here
	- minikube (FREE) --- click here
		○ Generally K8s cluster required minimum of two nodes(1 worker node and 1 Control plane) but testing purpose we use minikube which is also free, So, Here worker node and Control plane both are same node
	- kubeadm
	- Best option : cloud provider ==== for multi nodes
		○ Civo 
			§ low cost
			§ Very faster than others
			
Kubernetes dashboard
	- minikube dashboard
minikube commands
	- minikube start	start minikube 
	minikube status	see the status of minikube (controlplane, kubelet, api-server, kubeconfig
	kubectl get pods 	get all the pods inside worker nodes
	kubectl get nodes	get all the nodes 
		NAME       STATUS   ROLES           AGE   VERSION
		minikube   Ready    control-plane   14m   v1.28.3
	minikube docker-env	
	
	
kubectl commands
Useful tools
Lens
Monokle
Kubescape
Datree
Teleport
Civo
Hands-on demo
CNCF landscape


![image](https://github.com/abhijitxroy/kubernetes/assets/161963891/84896e91-3840-4c81-87a3-deb91c6bb277)
