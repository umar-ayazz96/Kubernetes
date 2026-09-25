INSTALLING AWS LOAD BALANCER CONTROLLER USING HELM

#################################################

aws iam get-role `
  --role-name AmazonEKSLoadBalancerControllerRole `
  --query "Role.Arn" `
  --output text

#################################################

eksctl create iamserviceaccount `
  --cluster <YOUR_CLUSTER_NAME> `
  --region ap-south-1 `
  --namespace kube-system `
  --name aws-load-balancer-controller `
  --attach-role-arn arn:aws:iam::<ACCOUNT_ID>:role/<YOUR_ROLE_NAME> `
  --override-existing-serviceaccounts `
  --approve

  ###############################################

helm upgrade --install aws-load-balancer-controller eks/aws-load-balancer-controller `
  -n kube-system `
  --set clusterName=shopsphere-cluster `
  --set serviceAccount.create=false `
  --set serviceAccount.name=aws-load-balancer-controller `
  --set region=ap-south-1 `
  --set vpcId=<YOUR_VPC_ID>

###############################################

Configure Argo CD for subpath:
server.rootpath: /argocd
server.basehref: /argocd

Because ALB is HTTP only, disable Argo CD's HTTPS redirect:
server.insecure: "true"

Edit with:
kubectl edit configmap argocd-cmd-params-cm -n argocd

Restart Argo CD:
kubectl rollout restart deployment argocd-server -n argocd
kubectl rollout status deployment argocd-server -n argocd
