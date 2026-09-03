# init
## Create talos controlplane
``` 
export CONTROL_PLANE_IP=192.168.1.40
talosctl gen config talos-proxmox-cluster https://$CONTROL_PLANE_IP:6443 --output-dir _out

```

## Create talos worker(s)
```
export WORKER_IP=192.168.1.50
talosctl apply-config --insecure --nodes $WORKER_IP --file worker.yaml
```

## Init talosctl
```
export TALOSCONFIG="talosconfig"
talosctl config endpoint $CONTROL_PLANE_IP
talosctl config node $CONTROL_PLANE_IP
talosctl bootstrap
```

## Init kubectl
```
talosctl kubeconfig .
export KUBECONFIG=kubeconfig

```

# Deploy app
```
kubectl apply -f https://raw.githubusercontent.com/siderolabs/example-workload/refs/heads/main/deploy/example-svc-nodeport.yaml
kubectl get pods,services # Lists the deployed pods and services
kubectl get svc example-workload -o jsonpath='{.spec.ports[0].nodePort}' # Get service "example-workload" IP address
```
