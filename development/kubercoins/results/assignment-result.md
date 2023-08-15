# Nombre de la asignación

Participantes
- Nombre

Diego Alejandro Trejo Angelino
Vicente Eduardo Escareño Lopez

Actividades
- Actividad

deploying-sample-application.sh

Comandos
- Comandos

Recursos externos
- Recurso externo

Artifactos
- Anexar artifactos al directorio de respuesta




-------------------- COMNANDOS -------------------------

[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ pwd
/home/k8s
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ ls -l
total 0
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ nano dockercoins.yaml
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ cat dockercoins.yaml 
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: hasher
  name: hasher
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hasher
  template:
    metadata:
      labels:
        app: hasher
    spec:
      containers:
      - image: dockercoins/hasher:v0.1
        name: hasher
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: hasher
  name: hasher
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: hasher
  type: ClusterIP
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis
  name: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - image: redis
        name: redis
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: redis
  name: redis
spec:
  ports:
  - port: 6379
    protocol: TCP
    targetPort: 6379
  selector:
    app: redis
  type: ClusterIP
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: rng
  name: rng
spec:
  replicas: 1
  selector:
    matchLabels:
      app: rng
  template:
    metadata:
      labels:
        app: rng
    spec:
      containers:
      - image: dockercoins/rng:v0.1
        name: rng
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: rng
  name: rng
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: rng
  type: ClusterIP
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: webui
  name: webui
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webui
  template:
    metadata:
      labels:
        app: webui
    spec:
      containers:
      - image: dockercoins/webui:v0.1
        name: webui
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: webui
  name: webui
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: webui
  type: NodePort
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: worker
  name: worker
spec:
  replicas: 1
  selector:
    matchLabels:
      app: worker
  template:
    metadata:
      labels:
        app: worker
    spec:
      containers:
      - image: dockercoins/worker:v0.1
        name: worker
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ kubectl apply -f dockercoins.yaml 
deployment.apps/hasher created
service/hasher created
deployment.apps/redis created
service/redis created
deployment.apps/rng created
service/rng created
deployment.apps/webui created
service/webui created
deployment.apps/worker created
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ pwd
/home/k8s
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ ls -l
total 4
-rw-r--r-- 1 k8s users 2256 Aug 15 00:43 dockercoins.yaml
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ docker image ls
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
hasher-65db859b66-9zn8s   0/1     Pending   0          4m57s
redis-6ff7cd7598-jzh9t    0/1     Pending   0          4m57s
rng-5bd86c8566-xbl7t      0/1     Pending   0          4m57s
webui-6969bf568c-8z2bh    0/1     Pending   0          4m57s
worker-6bbc87d469-86zz7   0/1     Pending   0          4m57s
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
hasher-65db859b66-9zn8s   0/1     Pending   0          6m16s
redis-6ff7cd7598-jzh9t    0/1     Pending   0          6m16s
rng-5bd86c8566-xbl7t      0/1     Pending   0          6m16s
webui-6969bf568c-8z2bh    0/1     Pending   0          6m16s
worker-6bbc87d469-86zz7   0/1     Pending   0          6m16s
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ kubectl get nodes
NAME    STATUS   ROLES           AGE    VERSION
test1   Ready    control-plane   121m   v1.27.4
[54.202.214.184] (kubernetes-admin@kubernetes:N/A) k8s@test1 ~
$ kubectl logs deploy/worker
[54.202.214.184] (kubernetes-admin@kubernetes:vicente) k8s@test1 ~
$ kubectl get nodes
NAME    STATUS   ROLES           AGE    VERSION
test1   Ready    control-plane   123m   v1.27.4
[54.202.214.184] (kubernetes-admin@kubernetes:vicente) k8s@test1 ~
$ kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
hasher-65db859b66-4ssq6   0/1     Pending   0          3m16s
redis-6ff7cd7598-m86x7    0/1     Pending   0          3m16s
rng-5bd86c8566-ppvcq      0/1     Pending   0          3m16s
webui-6969bf568c-28g28    0/1     Pending   0          3m16s
worker-6bbc87d469-d8wcl   0/1     Pending   0          3m16s
[54.202.214.184] (kubernetes-admin@kubernetes:vicente) k8s@test1 ~
$ kubectl logs deploy/hasher
[54.202.214.184] (kubernetes-admin@kubernetes:vicente) k8s@test1 ~
$ kubectl config set-context --current --namespace=default
Context "kubernetes-admin@kubernetes" modified.
[54.202.214.184] (kubernetes-admin@kubernetes:default) k8s@test1 ~
$ kubectl get svc webui
NAME    TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
webui   NodePort   10.104.161.179   <none>        80:32089/TCP   16m
[54.202.214.184] (kubernetes-admin@kubernetes:default) k8s@test1 ~
$ curl 10.104.161.179:80
curl: (7) Failed to connect to 10.104.161.179 port 80 after 0 ms: Connection refused
[54.202.214.184] (kubernetes-admin@kubernetes:default) k8s@test1 ~
$ curl 10.104.161.179:32089
^C