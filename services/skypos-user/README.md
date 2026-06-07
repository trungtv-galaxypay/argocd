# Skypos User

## EKS Uat
- ecr secret:
```
PASSWORD=$(aws ecr get-login-password --region ap-southeast-1)

kubectl create secret docker-registry ecr-secret \
--docker-server=941377160775.dkr.ecr.ap-southeast-1.amazonaws.com \
--docker-username=AWS \
--docker-password="$PASSWORD"
```

- run :
```sh
helm install skypos-user .
helm upgrade --install skypos-user . -n skypos
```

## Local
```
ducnp@GalaxyPays-MBP skypos-user % helm install skypos-user .     
NAME: skypos-user
LAST DEPLOYED: Sun May 31 21:37:55 2026
NAMESPACE: skypos
STATUS: deployed
REVISION: 1
DESCRIPTION: Install complete
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace skypos -l "app.kubernetes.io/name=skypos-user,app.kubernetes.io/instance=skypos-user" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace skypos $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace skypos port-forward $POD_NAME 8080:$CONTAINER_PORT
```

- create secret:
```
ducnp@GalaxyPays-MBP skypos-user % PASSWORD=$(aws ecr get-login-password --region ap-southeast-1)
ducnp@GalaxyPays-MBP skypos-user % kubectl create secret docker-registry ecr-secret \
--docker-server=123456789012.dkr.ecr.ap-southeast-1.amazonaws.com \
--docker-username=AWS \
--docker-password="$PASSWORD" \
-n skypos
secret/ecr-secret created
```

aws ecr describe-images \
--repository-name ecr-apse1-cicdgpay/dev/skypos/user \
--region ap-southeast-1

aws: [ERROR]: An error occurred (RepositoryNotFoundException) when calling the DescribeImages operation: The repository with name 'xxx.dkr.ecr.ap-southeast-1.amazonaws.com/xxy/dev/xx/usexxr' does not exist in the registry with id 'xxx'