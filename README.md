# awscli-eksctl

      install kubectl and eksctl and environment variable setup for eksctl & kubectl
      check if eksctl & kubectl installed properly using below command,
              eksctl --version
              kubectl --version
      
      create eks cluster with file
      
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

          

# if its failed in kubeconfig, update using below command for the cluster,

![image](https://user-images.githubusercontent.com/54719289/115592281-ed1b1280-a2ca-11eb-9d77-354b8dd35f6b.png)


        aws eks update-kubeconfig --name EKS-cluster  

![image](https://user-images.githubusercontent.com/54719289/115592117-c3fa8200-a2ca-11eb-9d0c-ac867d978537.png)

![image](https://user-images.githubusercontent.com/54719289/115592350-0623c380-a2cb-11eb-8f24-7cac77eb8e27.png)

