# Cloudwatch :
   
   # Cloudwatch Logs:

   eksctl utils update-cluster-logging --config-file clustercreate-cloudwatch.yml --approve
   
![image](https://user-images.githubusercontent.com/54719289/115937374-bf32fb00-a48f-11eb-9dc3-6705505eba82.png)

![image](https://user-images.githubusercontent.com/54719289/115937421-e984b880-a48f-11eb-84d0-d10a3080cc4e.png)
![image](https://user-images.githubusercontent.com/54719289/115937519-11741c00-a490-11eb-9e1a-301e60ae29d4.png)



 # To delete the cloudwatch logging:
 
    eksctl utils update-cluster-logging --name=EKS-cluster --disable-types all
 
 ![image](https://user-images.githubusercontent.com/54719289/115937847-d1616900-a490-11eb-8c58-f2a42a722c97.png)

    eksctl utils update-cluster-logging --name=EKS-cluster --disable-types all --approve
   


## Cloudwatch Agent(cloudwatch metrics)   :

       eksctl create cluster  -f clustercreate-cloudwatch.yml
       eksctl get cluster
       aws eks update-kubeconfig --name EKS-cluster
       kubectl get nodes
       kubectl get pods
       
![image](https://user-images.githubusercontent.com/54719289/115970201-d7ab2000-a538-11eb-81ab-b1e27e6661e5.png)

![image](https://user-images.githubusercontent.com/54719289/115970228-f90c0c00-a538-11eb-8af7-eff60965e80d.png)

![image](https://user-images.githubusercontent.com/54719289/115970237-09bc8200-a539-11eb-8938-4e654c1d302b.png)

![image](https://user-images.githubusercontent.com/54719289/115970307-69b32880-a539-11eb-90e2-705ae4c04643.png)

![image](https://user-images.githubusercontent.com/54719289/115970324-851e3380-a539-11eb-9e9e-b301c3ddc536.png)


# Check if this policy is attached  in all nodes

![image](https://user-images.githubusercontent.com/54719289/115970405-ffe74e80-a539-11eb-820f-ad78076f1e9a.png)


## deploy the cloudwatch agent
runs as daemonset, means one per node

```bash
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-yaml-templates/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/EKS-course-cluster/;s/{{region_name}}/eu-west-2/" | kubectl apply -f 


![image](https://user-images.githubusercontent.com/54719289/115970497-97e53800-a53a-11eb-9689-f98b6671c8d5.png)



  
