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

	kubectl get pods
	kubectl get nodes
	kubectl get nodes -l instance-type=spot

image](https://user-images.githubusercontent.com/54719289/115933344-9ce8af80-a486-11eb-9a92-8a5020f15836.png)



	kubectl scale --replicas=3 deployment/test-autoscaler
	kubectl get nodes
	kubectl get nodes -l instance-type=spot
	
![image](https://user-images.githubusercontent.com/54719289/115933853-ae7e8700-a487-11eb-81c9-1e7900ae0475.png)


### scale the deployment as 4 to check autoscaler

```bash
kubectl scale --replicas=4 deployment/test-autoscaler
```

### check pods

```bash
kubectl get pods -o wide --watch
```

### check nodes 

```bash
kubectl get nodes
```

![image](https://user-images.githubusercontent.com/54719289/115934096-2d73bf80-a488-11eb-9c03-1356b57d1d86.png)

![image](https://user-images.githubusercontent.com/54719289/115935303-cf94a700-a48a-11eb-9808-86bd1b851be0.png)

	kubectl scale --replicas=6 deployment/test-autoscaler
	kubectl get pods
	kubectl get nodes -l instance-type=spot


![image](https://user-images.githubusercontent.com/54719289/115936414-12f01500-a48d-11eb-85db-9c936bf0c71d.png)

### view cluster autoscaler logs

```bash```
	kubectl -n kube-system logs deployment.apps/cluster-autoscaler | grep -A5 "Expanding Node Group"

![image](https://user-images.githubusercontent.com/54719289/115936872-526b3100-a48e-11eb-8dc4-d35bb4fffe44.png)




	kubectl -n kube-system logs deployment.apps/cluster-autoscaler | grep -A5 "removing node"

![image](https://user-images.githubusercontent.com/54719289/115936583-8eea5d00-a48d-11eb-8823-9bf04a0cdda3.png)




