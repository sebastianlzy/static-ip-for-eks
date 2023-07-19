## Demo on host network (Ingress) using static IP

### Pre-requisite
1. Attached a EIP to an ENI

### Deploy nginx

```yaml
kubectl apply -f nginx-service-using-nodeport.yaml
```

### Invoke nginx endpoint

```yaml
curl <Static IP>
```

## Demo on host network (Egress) using static IP

### Add label to nodes

```
kubectl label nodes ip-<ADDRESS>.ap-southeast-1.compute.internal staticip=true
```

### Validate node with right label

```
kubectl get nodes --selector staticip=true                                                                          
NAME                                                STATUS   ROLES    AGE     VERSION
<ADDRESS>.ap-southeast-1.compute.internal   Ready    <none>   5h51m   v1.22.17-eks-0a21954

```

### Deploy container to the specific node
https://hub.docker.com/r/nicolaka/netshoot

```
kubectl run tmp-shell --rm -i --tty --overrides='{"spec": {"hostNetwork": true, "nodeSelector": {"staticip": "true"}}}' --image nicolaka/netshoot -- /bin/bash
```
```
# curl ifconfig.me
<Static IP>
```
