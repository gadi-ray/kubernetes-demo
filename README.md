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
