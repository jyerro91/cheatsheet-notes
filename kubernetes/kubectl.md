# Main Kube Commands
---

## CRUD
---

### Create Deployment
```bash
kubectl delete deployment $NAME
```

### Update Deployment
```bash
kubectl edit deployment $NAME
```

### Delete Deployment
```bash
kubectl delete deployment $NAME
```

### Delete all resources in the default namespace
```bash
kubectl delete all --all
```

### Delete all from certain namespace
```bash
kubectl delete all --all -n $NAMESPACE
```

### Delete and Recreate from certain namespace
```bash
kubectl delete namespace $NAMESPACE; kubectl create namespace $NAMESPACE
# or
kubectl delete "$(kubectl api-resources --namespaced=true --verbs=delete -o name | tr "\n" "," | sed -e 's/,$//')" --all
```

### Apply a configuration file
```bash
kubectl apply -f $MANIFEST_FILE/$MANIFEST_FOLDER_PATH
```

### Delete a configuration file                               
```bash
kubectl delete -f $MANIFEST_FILE/$MANIFEST_FOLDER_PATH
```

## Debugging Pods
---

### Log to console
```bash
kubectl logs $PODNAME
```

### Get interactive terminal
```bash
kubectl exec -it $PODNAME -- /bin/bash
```

### Get info about pod
```bash
kubectl describe pod $PODNAME
```

### Get more info about pod
```bash
kubectl get pod -o wide
```

### Get status of diff components
```bash
kubectl get {nodes,pod,services,deployment,replicaset}
```

### Get latest status from etcd
```bash
kubectl get deployment $DEPLOYMENT_NAME -o yaml > filename.yml
```
 