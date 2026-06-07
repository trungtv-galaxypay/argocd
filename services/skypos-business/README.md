# Skypos Business


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
helm install skypos-business .  -n skypos
helm upgrade --install skypos-business .  -n skypos
```

## Local
```text
NAME: skypos-business
LAST DEPLOYED: Mon Jun  1 08:57:11 2026
NAMESPACE: skypos
STATUS: deployed
REVISION: 1
DESCRIPTION: Install complete
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace skypos -l "app.kubernetes.io/name=skypos-business,app.kubernetes.io/instance=skypos-business" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace skypos $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace skypos port-forward skypos-business-787fb65ddc-cszjr 8080:8088
```

- upgrade
helm upgrade --install skypos-business .