Deploying cluster with AWS EKS
step-1 
Pre-requistes-
Make sure below tools are installed 
1-aws cli 2-kubectl 3-aws iam authenticator

step-1-creating an eks role
step-2-crating a vpc for eks
step-3 creating the EKS cluster
aws eks --region us-east-1 create-cluster --name demo --role-arnarn:aws:iam::011173820421:role/eksServiceRole --resources-vpc-config subnetIds=subnet-06d3631efa685f604,subnet-0f435cf42a1869282,subnet-03c954ee389d8f0fd,securityGroupIds=sg-0f45598b6f9aa110a



step-launching kubernetes worker nodes:-

open cloud formation create stack and use the below url for the template
https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-01-09/amazon-eks-nodegroup.yaml

    ClusterName – the name of your Kubernetes cluster (e.g. demo)
    ClusterControlPlaneSecurityGroup – the same security group you used for creating the cluster in previous step.
    NodeGroupName – a name for your node group.
    NodeAutoScalingGroupMinSize – leave as-is. The minimum number of nodes that your worker node Auto Scaling group can scale to.
    NodeAutoScalingGroupDesiredCapacity – leave as-is. The desired number of nodes to scale to when your stack is created.
    NodeAutoScalingGroupMaxSize – leave as-is. The maximum number of nodes that your worker node Auto Scaling group can scale out to.
    NodeInstanceType – leave as-is. The instance type used for the worker nodes.
    NodeImageId – the Amazon EKS worker node AMI ID for the region you’re using. For us-east-1, for example: ami-0c5b63ec54dd3fc38
    KeyName – the name of an Amazon EC2 SSH key pair for connecting with the worker nodes once they launch.
    BootstrapArguments – leave empty. This field can be used to pass optional arguments to the worker nodes bootstrap script.
    VpcId – enter the ID of the VPC you created in Step 2 above.
    Subnets – select the three subnets you created in Step 2 above. 

stack details


step-5-->Step 5: Installing a demo app on Kubernetes

he following commands create the different Kubernetes building blocks required to run the app — the Redis master replication controller, the Redis master service, the Redis slave replication controller, the Redis slave service, the Guestbook replication controller and the guestbook service itself:

kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/redis-master-controller.json

kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/redis-master-service.json

kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/redis-slave-controller.json

kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/redis-slave-service.json

kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/guestbook-controller.json

kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/guestbook-service.json 

Use kubectl to see a list of your services:

k

ubectl get svc

guestbook      LoadBalancer   10.100.17.29     aeeb4ae3132ac11e99a8d12b26742fff-1272962453.us-east-1.elb.amazonaws.com   3000:31747/TCP   7m
kubernetes     ClusterIP      10.100.0.1       <none>                                                                    443/TCP          1h
redis-master   ClusterIP      10.100.224.82    <none>                                                                    6379/TCP         8m
redis-slave    ClusterIP      10.100.150.193   <none>                                                                    6379/TCP         8m