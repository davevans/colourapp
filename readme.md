# Initialise empty Helm chart
```
helm create colour-app

# delete default templates
rm -rf ./colour-app/templates/*
```

# Package helm chart
```
# Execute from directory containing chart.yaml
# https://helm.sh/docs/helm/helm_package/

helm package . --version 1.0.0
```

# Push to ECR
https://docs.aws.amazon.com/AmazonECR/latest/public/push-oci-artifact.html
 
```

# create a repository for chart. NB: region has to be us-east-1 for public ECR
aws ecr-public create-repository --repository-name colour-app --region us-east-1

# authenticate
aws ecr-public get-login-password \
     --region us-east-1 | helm registry login \
     --username AWS \
     --password-stdin public.ecr.aws

# push the chart. NB: alias of Dav public registry is "j8q5k6b9"
helm push colour-app-1.0.0.tgz oci://public.ecr.aws/j8q5k6b9

# describe images
aws ecr-public describe-images --repository-name colour-app --region us-east-1

```