# eks
To get details about the current IAM identity

The following get-caller-identity example displays information about the IAM identity used to authenticate the request. The caller is an IAM user.

aws sts get-caller-identity
Output:

{
    "UserId": "AIDASAMPLEUSERID",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/DevAdmin"
}


Create an IAM role for manage EKS and assign to Cloud9 Ec2 instance

https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html

Install eksctl
To install or upgrade eksctl on Linux using curl

Download and extract the latest release of eksctl with the following command.

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
Move the extracted binary to /usr/local/bin.

sudo mv /tmp/eksctl /usr/local/bin
Test that your installation was successful with the following command.

eksctl version


Install kubectl

To install kubectl on Linux

Download the Amazon EKS vended kubectl binary for your cluster's Kubernetes version from Amazon S3. To download the Arm version, change amd64 to arm64 before running the command.

Kubernetes 1.21:

curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl

Apply execute permissions to the binary.

chmod +x ./kubectl

Copy the binary to a folder in your PATH. If you have already installed a version of kubectl, then we recommend creating a $HOME/bin/kubectl and ensuring that $HOME/bin comes first in your $PATH.

mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin

After you install kubectl, you can verify its version with the following command:

kubectl version --short --client

as next step- Create EKS cluster using the following command
eksctl create cluster -f eks.yaml 

kubectl get nodes

You can add one or more nodegroups in addition to the initial nodegroup created along with the cluster.

To create an additional nodegroup, use:


eksctl scale nodegroup --cluster=eks-demo --name=Eks-Node --nodes=2 --nodes-min=2 --nodes-max=3 --region us-west-2

------------------
kubectl get clusterroles
kubectl get clusterroles cluster-admin -o yaml
kubectl get clusterrolebindings
kubectl get clusterrolebindings cluster-admin -o yaml
kubectl get roles -n kube-system
kubectl get rolebindings -n kube-system

Now user binding step

kubectl get clusterroles view -o yaml

kubectl apply -f userbin.yaml

Create a IAM user after that map this user to K8S

kubectl edit cm aws-auth -n kube-system
(configmap- cm)

  mapUsers: |
      - userarn: arn:aws:iam::1234567890:user/xxx-eks-user
        username: dev
        
 q! exit without saving !
 
 2nd way to do it
 eksctl get iamidentitymapping --cluster eks-demo --region us-west-2
 
 eksctl create iamidentitymapping --cluster eks-demo --region us-west-2 --arn arn:aws:iam::083892641784:user/finbourne-eks-user --username dev
 kubectl get cm aws-auth -n kube-system -o yaml
 
 aws configure --profile eksdev
 
 aws eks update-kubeconfig --name eks-demo --profile eksdev
 
 kubectl get svc
 kubectl get nodes  - Error
 

For ROLES (!!!!)

eksctl create iamidentitymapping --cluster eks-demo --region us-west-2 --arn arn:aws:iam::rolearn --username system:serviceaccount:kube-system:dev --group system:serviceaccount:kube-system
kubectl get cm aws-auth -n kube-system -o yaml

NOTE: Delete mapUsers
eksctl delete iamidentitymapping --cluster eks-demo --arn userarn --region us-west-2

go to powershell and

aws eks update-kubeconfig --name eks-demo --role-arn arn:roleyoucreated --profile eksdev



------------------------------------------

For security to access EKS

 aws eks update-kubeconfig --name eks-demo --role-arn arn:aws:sts::083892641784:assumed-role/EKS-demo-1-role --profile eksdemo --region us-west-2
 





