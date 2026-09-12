#####creating service account for AWS Load Balancer Controller

eksctl create iamserviceaccount `
  --cluster <YOUR_CLUSTER_NAME> `
  --region ap-south-1 `
  --namespace kube-system `
  --name aws-load-balancer-controller `
  --attach-role-arn arn:aws:iam::<ACCOUNT_ID>:role/<YOUR_ROLE_NAME> `
  --override-existing-serviceaccounts `
  --approve
