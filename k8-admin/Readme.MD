```
# aws-auth is a config map where IAM and EKS are integrated.
# to list the aws-auth file
# kubectl get configmap aws-auth -n kube-system -o yaml 
# to update
# kubectl update confimap aws-auth -n kube-system -o yaml

# Iam is used for authentacation
# authorizatioon is handled by EKS 

# create user
# create policy to describe the pod
# create ARN as well
# attach the policy to the user

# crate role using below file 
# kubectl apply -f 01-role.yaml
# kubectl get roles -n roboshop


# create role bindng and bide the user with that role
# kubectl apply -f 02-rolebinding.yaml
# kubectl get rolebindings

## how eks know that the user should take from IAM
# by using the aws-auth file 
# kubectl get configmap aws-auth -n kube-system -o yaml


## edit this file and run update command 
# eksctl update configmap aws-auth -n kube-system -o yaml

# once it is done the user can login to there ec2 instance with there access keys and perfoem eks cluster describe actions


# it will make a kube config file for authentication once you entered the above command
# kubectl update configmap aws-auth -n kube-system -o yaml


# to get caller identity
# aws sks get-caller-identity

# create groups in aws-auth and give the role roboshop-trainee this will go and match with the roboshop-trainee group and the users in this group will get the same access



##########

```
for system user

kubernetes will create default sa account while creating


#########
to install the openid connect which help us to connect tthe pod to outside resources.

eksctl utils associate-iam-oidc-provider --cluster roboshop-dev --approve


Create policy to access the secrete manager

give secret arn


SA -> Role -> Attach policy to the role


 create sa and role

eksctl create iamserviceaccount --cluster roboshop-dev --name secret-reader --namespace roboshop --attach-policy-arn arn:aws:iam::160885265516:policy/RoboShopMySQLSecretReader --approve

kubectl get sa secret-reader -o yaml

get the code for sa.yaml

anotations are used to select the resources out side of the ek cluster

to test we need awscli
create pod with image aws cli
and test the service account
and mention ServiceAccount name create as below in the container level in pod.yaml
ServiceAccount: roboshop

```

aws secretsmanager get-secret-value --secret-id roboshop/dev/mysql_password --query SecretString --output text

```



```

##########
```


troubleshooting:

```
aws cloudformation describe-stack-events   --stack-name eksctl-roboshop-nodegroup-roboshop   --region us-east-1   --query 'StackEvents[].[Timestamp,LogicalResourceId,ResourceType,ResourceStatus,ResourceStatusReason]'   --output table
```

```
aws eks describe-nodegroup   --cluster-name roboshop   --nodegroup-name roboshop   --region us-east-1   --query 'nodegroup.{Status:status,Reason:health.issues,StatusReason:statusReason,Desired:scalingConfig.desiredSize,Min:scalingConfig.minSize,Max:scalingConfig.maxSize,InstanceType:instanceTypes,Subnets:subnets}'   --output json
```

```
 aws cloudformation describe-stacks   --stack-name eksctl-roboshop-nodegroup-roboshop   --region us-east-1   --query 'Stacks[0].[StackStatus,StackStatusReason]'   --output table

```
