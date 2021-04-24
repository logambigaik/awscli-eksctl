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
       kubectl get all amazon-cloudwatch
       kubectl get all -n amazon-cloudwatch

![image](https://user-images.githubusercontent.com/54719289/115970201-d7ab2000-a538-11eb-81ab-b1e27e6661e5.png)

![image](https://user-images.githubusercontent.com/54719289/115970228-f90c0c00-a538-11eb-8af7-eff60965e80d.png)

![image](https://user-images.githubusercontent.com/54719289/115970237-09bc8200-a539-11eb-8938-4e654c1d302b.png)

![image](https://user-images.githubusercontent.com/54719289/115970307-69b32880-a539-11eb-90e2-705ae4c04643.png)

![image](https://user-images.githubusercontent.com/54719289/115970324-851e3380-a539-11eb-9e9e-b301c3ddc536.png)

![image](https://user-images.githubusercontent.com/54719289/115972904-26f94c80-a549-11eb-8a69-869d4207ef6a.png)


# Check if this policy is attached  in all nodes

![image](https://user-images.githubusercontent.com/54719289/115970405-ffe74e80-a539-11eb-820f-ad78076f1e9a.png)


## deploy the cloudwatch agent
runs as daemonset, means one per node

         ClusterName='EKS-cluster'
         LogRegion='us-west-2'
         FluentBitHttpPort='2020'
         FluentBitReadFromHead='Off'
         [[ ${FluentBitReadFromHead} = 'On' ]] && FluentBitReadFromTail='Off'|| FluentBitReadFromTail='On'
         [[ -z ${FluentBitHttpPort} ]] && FluentBitHttpServer='Off' || FluentBitHttpServer='On'
         curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluent-bit-quickstart.yaml | sed 's/{{cluster_name}}/'${ClusterName}'/;s/{{region_name}}/'${LogRegion}'/;s/{{http_server_toggle}}/"'${FluentBitHttpServer}'"/;s/{{http_server_port}}/"'${FluentBitHttpPort}'"/;s/{{read_from_head}}/"'${FluentBitReadFromHead}'"/;s/{{read_from_tail}}/"'${FluentBitReadFromTail}'"/' | kubectl apply -f - 



![image](https://user-images.githubusercontent.com/54719289/115970497-97e53800-a53a-11eb-9689-f98b6671c8d5.png)

          kubectl get all  amazon-cloudwatch

          kubectl get all -n amazon-cloudwatch
      
![image](https://user-images.githubusercontent.com/54719289/115973163-e00c5680-a54a-11eb-841f-df0b69dfe1ab.png)

![image](https://user-images.githubusercontent.com/54719289/115973803-b1dd4580-a54f-11eb-8d69-0eadb6b18679.png)



# Delete the cloudwatch agent:

curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/EKS-course-cluster/;s/{{region_name}}/us-east-1/" | kubectl delete -f -




## load generation

```bash
         kubectl run php-apache --image=k8s.gcr.io/hpa-example --requests=cpu=200m --limits=cpu=500m --expose --port=80
```      

```bash
         kubectl run --generator=run-pod/v1 -it --rm load-generator --image=busybox /bin/sh

Hit enter for command prompt

while true; do wget -q -O- http://php-apache.default.svc.cluster.local; done
