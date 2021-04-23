# Cloudwatch :


   eksctl utils update-cluster-logging --config-file clustercreate-cloudwatch.yml --approve
   
![image](https://user-images.githubusercontent.com/54719289/115937374-bf32fb00-a48f-11eb-9dc3-6705505eba82.png)

![image](https://user-images.githubusercontent.com/54719289/115937421-e984b880-a48f-11eb-84d0-d10a3080cc4e.png)
![image](https://user-images.githubusercontent.com/54719289/115937519-11741c00-a490-11eb-9e1a-301e60ae29d4.png)



 # To delete the cloudwatch logging:
 
    eksctl utils update-cluster-logging --name=EKS-cluster --disable-types all
 
 ![image](https://user-images.githubusercontent.com/54719289/115937847-d1616900-a490-11eb-8c58-f2a42a722c97.png)

    eksctl utils update-cluster-logging --name=EKS-cluster --disable-types all --approve
   


## Cloudwatch Agent:


  
