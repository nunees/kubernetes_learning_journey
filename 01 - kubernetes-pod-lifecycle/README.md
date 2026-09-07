# Projeto 1 - Laboratório de Ciclo de Vida de Pods

**Objetivo: praticar criação, inspeção, logs e exclusão.**

Desafios:
- Criar um Pod Nginx via comando.
- Criar o mesmo Pod via YAML.
- Visualizar detalhes com kubectl describe.
- Acessar logs.
- Entrar no container com kubectl exec.
- Excluir e recriar o Pod.

## Resolução

1 - Criar um Pod Nginx via comando
```shell
  kubectl run nginx --image nginx --port=80
```

2 - Criar o mesmo Pod via YAML.

**nginx-pod.yaml**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx
  labels:
    run: my-nginx
spec:
  containers:
    - name: my-nginx
      image: nginx
      ports:
        - containerPort: 80
```

```shell
  kubectl create -f nginx-pod.yaml
```

3 - Visualizar detalhes com kubectl describe 
```shell
  kubectl describe pods my-nginx
```

```shell
Name:             my-nginx
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Sun, 06 Sep 2026 21:39:24 -0300
Labels:           run=my-nginx
Annotations:      <none>
Status:           Running
IP:               10.244.0.15
IPs:
  IP:  10.244.0.15
Containers:
  my-nginx:
    Container ID:   containerd://8885f6d1499ea0d956d091371234bfca576980a179ed06c7e6968fb452fd4721
    Image:          nginx
    Image ID:       docker.io/library/nginx@sha256:05b8cb60c354a44ab824ea6e7dc69b46d50762cdbe728a347a5b656e6fb3d7c4
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sun, 06 Sep 2026 21:39:25 -0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-4k4xh (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-4k4xh:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
```

4 - Acessar logs.

```shell
  kubectl logs my-nginx
```

```shell
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/09/07 00:39:25 [notice] 1#1: using the "epoll" event method
2026/09/07 00:39:25 [notice] 1#1: nginx/1.31.5
2026/09/07 00:39:25 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19) 
2026/09/07 00:39:25 [notice] 1#1: OS: Linux 6.10.14-linuxkit
2026/09/07 00:39:25 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/09/07 00:39:25 [notice] 1#1: start worker processes
2026/09/07 00:39:25 [notice] 1#1: start worker process 29
2026/09/07 00:39:25 [notice] 1#1: start worker process 30
2026/09/07 00:39:25 [notice] 1#1: start worker process 31
2026/09/07 00:39:25 [notice] 1#1: start worker process 32
2026/09/07 00:39:25 [notice] 1#1: start worker process 33
2026/09/07 00:39:25 [notice] 1#1: start worker process 34
2026/09/07 00:39:25 [notice] 1#1: start worker process 35
2026/09/07 00:39:25 [notice] 1#1: start worker process 36
```

5 - Entrar no container com kubectl exec.

```shell
  kubectl exec -it my-nginx -- sh 
```

6 - Excluir e recriar o Pod

```shell
  kubectl delete pods my-nginx 
```

```shell
  kubectl create -f nginx-pod.yaml 
```