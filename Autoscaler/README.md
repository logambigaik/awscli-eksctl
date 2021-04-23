# README

![image](https://user-images.githubusercontent.com/54719289/115865363-1acd9c00-a430-11eb-810a-64806d7ef8ae.png)

![image](https://user-images.githubusercontent.com/54719289/115865263-fffb2780-a42f-11eb-9204-1d4c498397b1.png)

![image](https://user-images.githubusercontent.com/54719289/115865695-97f91100-a430-11eb-9687-428711a3e44c.png)

# Deploy the Cluster Autoscaler.

kubectl apply -f https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml


Step1 : Annotate the cluster-autoscaler service account with the ARN of the IAM role that you created previously. Replace the <example values> with your own values.
  eks.amazonaws.com/role-arn=arn:aws:iam::136962450893:role/eks_createIAM_role
	
   kubectl annotate serviceaccount cluster-autoscaler \
           -n kube-system \
           eks.amazonaws.com/role-arn=arn:aws:iam::136962450893:role/eks_createIAM_role


Step2 : Patch the deployment to add the cluster-autoscaler.kubernetes.io/safe-to-evict annotation to the Cluster Autoscaler pods with the following command.

    kubectl patch deployment cluster-autoscaler \
            -n kube-system \
            -p '{"spec":{"template":{"metadata":{"annotations":{"cluster-autoscaler.kubernetes.io/safe-to-evict":             "false"}}}}}'


Step3: Open the Cluster Autoscaler deployment with the following command.

      kubectl -n kube-system edit deployment.apps/cluster-autoscaler


Step4 : Edit the cluster-autoscaler container command to replace <YOUR CLUSTER NAME> (including <>) with your cluster's name, and add the following options.

--balance-similar-node-groups

--skip-nodes-with-system-pods=false


Step5 : Set the Cluster Autoscaler image tag to the version that you recorded in the previous step with the following command. Replace 1.19.n with your own value

     kubectl set image deployment cluster-autoscaler \
     -n kube-system \
     cluster-autoscaler=k8s.gcr.io/autoscaling/cluster-autoscaler:v1.20.0


Step6 :View Cluster logs:

      kubectl -n kube-system logs -f deployment.apps/cluster-autoscaler
![image](https://user-images.githubusercontent.com/54719289/115931307-d28b9980-a482-11eb-92fd-69afccd6070a.png)


## test the autoscaler

	apiVersion: extensions/v1beta1
	kind: Deployment
	metadata:
  		name: test-autoscaler
	spec:
  		replicas: 1
  	template:
    		metadata:
      			labels:
        			service: nginx
        			app: nginx
    	spec:
      		containers:
      			- image: nginx
        	          name: test-autoscaler
        	resources:
          		limits:
            			cpu: 300m
            			memory: 512Mi
          		requests:
            			cpu: 300m
            			memory: 512Mi
      		nodeSelector:
        		instance-type: spot



### create a deployment of nginx

```bash
kubectl apply -f nginx-deployment.yaml
```

### scale the deployment

```bash
kubectl scale --replicas=3 deployment/test-autoscaler
```

### check pods

```bash
kubectl get pods -o wide --watch
```

### check nodes 

```bash
kubectl get nodes
```

### view cluster autoscaler logs

```bash
kubectl -n kube-system logs deployment.apps/cluster-autoscaler | grep -A5 "Expanding Node Group"

kubectl -n kube-system logs deployment.apps/cluster-autoscaler | grep -A5 "removing node"
```




