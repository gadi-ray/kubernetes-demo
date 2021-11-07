# Kubernetes step by step tutorial

## Step 0 - Enable Kubernetes
![alt text](k8s.png "k8s enabled")

## Step 1 - Pod
```kubectl apply -f pod.yaml```

```kubectl get pods```

```kubectl port-forward nginx-pod 8081:80```

Go to http://localhost:8081

```kubectl delete -f pod.yaml```


## Step 2 - Replica Set

```kubectl apply -f rs.yaml```

```kubectl get rs```

```kubectl get pods```

```kubectl delete pod nginx-replica-set-<POD_ID>```

```kubectl get pods```

```kubectl scale rs nginx-replica-set --replicas=5```

```kubectl get pods```

```kubectl scale rs nginx-replica-set --replicas=3```

```kubectl get pods```

```kubectl apply -f pod.yaml```

```kubectl get pods```

```kubectl delete -f rs.yaml```

## Step 3 - Deployment

```kubectl apply -f deployment.yaml```

```kubectl get pods```

```kubectl get rs```

```kubectl get deployments```

```kubectl scale deployments/nginx-deployment --replicas=5```

```kubectl get pods```

```kubectl set image deployments/nginx-deployment nginx=bmoshe/static-content-per-pod:latest --record```

```kubectl rollout history deployment/nginx-deployment```

```kubectl get pods```

```kubectl port-forward nginx-deployment-<POD_ID> 8081:80```

Go to http://localhost:8081 and see the new nginbix image

```kubectl rollout undo deployment/nginx-deployment```

```kubectl rollout history deployment/nginx-deployment```

```kubectl get pods```

```kubectl port-forward nginx-deployment-<POD_ID> 8081:80```

Go to http://localhost:8081 and verify the rollback worked

```kubectl delete -f deployment.yaml```

## Step 4 - Service

```kubectl apply -f deployment.yaml -f service.yaml```

```kubectl get services```

```kubectl describe service nginx-service```

Go to http://localhost:31000

```kubectl set image deployments/nginx-deployment nginx=bmoshe/static-content-per-pod:latest --record```

```kubectl scale deployments/nginx-deployment --replicas=5```

Visit http://localhost:31000 multiple times from incognito to verify multiple pods are called

```kubectl delete -f deployment -f service.yaml```

## Step 5 - Ingress

```kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/cloud/deploy.yaml```

```kubectl get pods --namespace=ingress-nginx```

```kubectl apply -f ./nginx/deployment.yaml -f ./nginbix/deployment.yaml```

```kubectl get pods```

```kubectl get services```

```kubectl apply -f nginx-ingress.yaml```

```kubectl get ingress```

```kubectl describe ingress nginx-ingress```

Visit http://localhost/nginx

Visit http://localhost/nginbix

Visit http://localhost/bla

```kubectl delete -f nginx-ingress.yaml```

```kubectl delete -f ./nginbix/deployment.yaml -f ./nginx/deployment.yaml```

## Step 6 - nginx Helm

```brew install helm```

```helm install nginx nginx-chart --dry-run```

```helm install nginx nginx-chart```

```helm list```

```kubectl apply -f nginx-ingress.yaml```

Visit http://localhost/nginx

```kubectl delete -f nginx-ingress.yaml```

```helm delete nginx```

## Step 6 - template Helm

```helm install nginx template-chart --dry-run```

```helm install nginx template-chart```

```helm install nginbix template-chart```

```helm list```

```helm history```

```helm upgrade nginbix template-chart --set replicaCount=5,image.repository=bmoshe/static-content-per-pod,image.tag="latest"```

```kubectl get pods```

```kubectl apply -f nginx-ingress.yaml```

Visit http://localhost/nginx

Visit http://localhost/nginbix

```helm rollback nginbix 1```

```helm history```

Visit http://localhost/nginbix


```helm delete nginx```

```helm delete nginbix```

```kubectl delete -f nginx-ingress.yaml```

## Step 6 - Helm namespaces

```kubectl create ns qa```

```kubectl create ns prod```

```kubectl get ns```

```helm package template-chart```

```helm install nginx template-chart-0.1.0.tgz --namespace=qa```

```helm history```

```helm history --namespace=qa```

```helm install nginx template-chart-0.1.0.tgz --namespace=prod```

```helm history --namespace=prod```

```helm upgrade nginx template-chart --set replicaCount=6,image.tag="1.21.0" --namespace=prod --dry-run```

```helm history --namespace=prod```

```helm upgrade nginx template-chart --set replicaCount=6,image.tag="1.21.0" --namespace=prod```

```helm history --namespace=prod```

```helm rollback nginx 1 --namespace=prod```

```helm history --namespace=prod```

```helm delete nginx --namespace=qa```

```helm delete nginx --namespace=prod``` 

```kubectl delete -f nginx-ingress.yaml```

```kubectl delete ns qa```

```kubectl delete ns prod```

