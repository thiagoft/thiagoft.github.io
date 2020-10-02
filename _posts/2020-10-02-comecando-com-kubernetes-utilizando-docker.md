---
layout: post
title: "Começando com Kubernetes utilizando Docker"
date: 2020-10-02 02:42:23 -0300
categories: Devops
---

# Começando com kubernetes e docker

Existem algumas formas de criar um cluster de kubernetes para testarmos principalmente localmente, para quem ja utiliza docker-desktop existe a opção em:
Preferences > Kubernetes > Enable Kubernetes
Automaticamente um cluster de Kubernetes será inicializado e então podemos seguir com este rápido quick-start.

Criando arquivo de configuração do primeiro Pod: nginx-pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
```

Executando o comando passando como parametro de -f o arquivo nginx-pod.yaml:

```bash
kubectl apply -f ./nginx-pod.yaml
-> pod/nginx created
```

Para verificar o Pod

```bash
kubectl get pod

NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          2m43s
```

Neste momento o Pod está criado porem ainda inacessível.

Para mais informações sobre o Pod podemos utilizar:

```bash
kubectl describe pod nginx
```

Dentre várias informações é informado o IP do Pod, porém é um IP interno do Cluster, não ligado diretamente ao Host, é necessário que seja feito um roteamento/exposição.

Neste caso então criamos mais um arquivo que definirá um serviço que irá descrever o roteamento para este pod, no caso chamaremos de nginx-service.yaml.

- O Kubernetes tem 3 tipos de "Services"

  1. ClusterIP - cria roteamentos dentro e somente dentro do cluster
  2. NodePort - cria roteamentos de fora para dentro do cluster
  3. LoadBalancer - cria um load balance para os serviços

  Um detalhe importante é que 2 atual também como 1 assim como 3 atual como 2 e 1.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  ports:
    - port: 80 # No caso de ClusterIp somente a definição da porta é o suficiente
      nodePort: 30000 # Exposição da porta 30000 redirecionando para a porta 80 do Pod
      # OBS: The range of valid ports is 30000-32767
  selector:
    app: web-server
```

```bash
kubectl apply -f ./nginx-service.yaml
-> service/nginx-service created
```

Para visualizarmos a execução do service:

```bash
kubectl get services

NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP        55m
nginx-service   NodePort    10.111.227.2   <none>        80:30000/TCP   2m49s
```

Vale notar que o ip para acesso externo está <none>, porém quando utilizamos o docker-desktop, é feito um bind para o [localhost](http://localhost) de forma automática. Então agora caso acessarmos:

```bash
http://localhost:30000
```

Será feito o redirecionamento para o Pod e então para a exposição de porta configurada, no caso a porta 80 para o nginx.

Porém vale lembrar que um Pod por si so não garante a escala, e a resiliência do serviço, é onde entram os Deployments, então podemos criar um arquivo chamado: nginx-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  template:
    metadata:
      name: nginx-pod
      labels:
        app: web-server
      spec:
        containers:
          - name: nginx-container
            image: nginx:latest
  selector:
    matchLabels:
      app: web-server
  replicas: 3
```

```bash
kubectl apply -f ./nginx-deplyment.yaml
-> deployment.apps/nginx-deployment created
```

Para vermos o resultado:

```bash
kubectl get pods

NAME                               READY   STATUS    RESTARTS   AGE
nginx                              1/1     Running   3          57m
nginx-deployment-f7d8fb454-88xhn   1/1     Running   3          5m16s
nginx-deployment-f7d8fb454-dl8kk   1/1     Running   3          5m16s
nginx-deployment-f7d8fb454-lk628   1/1     Running   3          5m16s
```

Podemos observar o Pod que criamos inicialmente e então as 3 novas réplicas que criamos via Deployment. Este recurso garantirá que todas as réplicas estejam sempre em execução, e como temos o Service ja configurado teremos o balanceamento da carga entre as réplicas.

[Repositório com os arquivos de configuração](https://github.com/thiagoft/kubernetes-quickstart).
