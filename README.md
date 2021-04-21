# awscli-eksctl

      install kubectl & eksctl and do the environment variable setup
      check if eksctl & kubectl installed properly,
              eksctl --version
              kubectl --version
      
  # create eks cluster with file
  
  # Command :  eksctl create cluster -f clustercreation.yml
            
      
        $ cat clustercreation.yml
        apiVersion: eksctl.io/v1alpha5
        kind: ClusterConfig

        metadata:
          name: EKS-cluster
          region: eu-west-2

        nodeGroups:
          - name: ng-1
              instanceType: t2.small
              desiredCapacity: 2
          ssh:
              publicKeyName: Archu-acc

          
            
# if its failed in kubeconfig, update kubeconfig for the cluster with below command,

      aws eks update-kubeconfig --name EKS-cluster  

![image](https://user-images.githubusercontent.com/54719289/115592281-ed1b1280-a2ca-11eb-9d77-354b8dd35f6b.png)


        

![image](https://user-images.githubusercontent.com/54719289/115592117-c3fa8200-a2ca-11eb-9d0c-ac867d978537.png)

![image](https://user-images.githubusercontent.com/54719289/115592350-0623c380-a2cb-11eb-8f24-7cac77eb8e27.png)


# eks cluster:

       eksctl get cluster

# eks nodeGroup:

          eksctl get nodegroup --cluster EKS-cluster

![image](https://user-images.githubusercontent.com/54719289/115593724-c5c54500-a2cc-11eb-98c7-34bf260b8299.png)


#  eksctl scale nodegroup --cluster=EKS-cluster --nodes=5 --name=ng-1

![image](https://user-images.githubusercontent.com/54719289/115597438-2d7d8f00-a2d1-11eb-98ff-ea0daf0bc6c8.png)


# $ eksctl create nodegroup --config-file=clustercreation.yml --include='ng-mixed'
![image](https://user-images.githubusercontent.com/54719289/115600037-25731e80-a2d4-11eb-942c-339272ed318d.png)





