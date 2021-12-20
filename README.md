# deploy-mon for Docker Compose

## Requirement

- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Usage

### Run containers using docker-compose

- [docker-compose.yml](https://github.com/RHEMS-Japan/deploy-mon/blob/main/docker-compose.yml)

- command
```
docker-compose up -d
```

- sample
```
(⎈ |docker-desktop:default)docker-compose up -d
Docker Compose is now in the Docker CLI, try `docker compose up`

Creating network "deploy-mon_default" with the default driver
Creating check-mon ... done
Creating dashboard ... done
Creating mongodb   ... done
(⎈ |docker-desktop:default)
```
```
(⎈ |docker-desktop:default)docker ps
CONTAINER ID   IMAGE                                     COMMAND                  CREATED          STATUS          PORTS                      NAMES
463ecaa1c2ee   rhemsjapan/blue-deploy-mon:admin-latest   "docker-entrypoint.s…"   20 seconds ago   Up 16 seconds   0.0.0.0:3000->3000/tcp     dashboard
95572128d197   rhemsjapan/blue-deploy-mon:api-latest     "waitress-serve --po…"   20 seconds ago   Up 13 seconds   0.0.0.0:5000->5000/tcp     check-mon
4eed469d3f2d   mongo:latest                              "docker-entrypoint.s…"   20 seconds ago   Up 14 seconds   0.0.0.0:27017->27017/tcp   mongodb
(⎈ |docker-desktop:default)
```


### Access the following URL in your browser

- URL

http://0.0.0.0:3000/


- sample image

![sample image](https://github.com/RHEMS-Japan/deploy-mon/blob/main/img/sample_image.png)


# deploy-mon for k8s

## Requirement


- [Docker Desktop](https://www.docker.com/products/docker-desktop) 
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Usage


### Create ymls 

- Use sample ymls in [k8s_sample](https://github.com/RHEMS-Japan/deploy-mon/tree/main/k8s_sample).
- Add value that the path where [initdb.d](https://github.com/RHEMS-Japan/deploy-mon/tree/main/k8s_sample/initdb.d) is located in [mongodb.yml#L42](https://github.com/RHEMS-Japan/deploy-mon/blob/main/k8s_sample/mongodb.yml#L42/).

### Run containers using kubectl

- Run it in the directory where you created [ymls](https://github.com/RHEMS-Japan/deploy-mon#create_ymls).


- command
```
kubectl apply -k .
```

- sample
```
(⎈ |docker-desktop:blue-deploy-check)kubectl apply -k .
namespace/blue-deploy-check created
serviceaccount/admin-deploy-checker created
clusterrolebinding.rbac.authorization.k8s.io/admin-clusterrolebinding-deploy-checker created
service/api-svc created
service/dashboard-svc created
service/mongo-svc created
deployment.apps/check created
deployment.apps/dashboard created
deployment.apps/mongodb created
(⎈ |docker-desktop:blue-deploy-check)
```
```
(⎈ |docker-desktop:blue-deploy-check)kubectl get pod
NAME                         READY   STATUS              RESTARTS   AGE
check-6f46c9b5b9-28xnp       0/1     ContainerCreating   0          4s
dashboard-59579f8d76-86qp6   0/1     ContainerCreating   0          4s
mongodb-774cd5f658-gsspq     0/1     ContainerCreating   0          4s
(⎈ |docker-desktop:blue-deploy-check)
```
### Check the target port for dashbord

- command
```
kubectl get svc dashboard-svc
```

- sample
```
(⎈ |docker-desktop:blue-deploy-check)kubectl get svc dashboard-svc
NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
dashboard-svc   NodePort   10.109.49.219   <none>        3000:31131/TCP   25m
(⎈ |docker-desktop:blue-deploy-check)
```
(In this case, the target port for dashbord is 31131.)

### Access the following URL in your browser

- URL

http://0.0.0.0: [<target_port_for_dashbord>](https://github.com/RHEMS-Japan/deploy-mon#check_the_target_port_for_dashbord) /

- sample image

![sample image](https://github.com/RHEMS-Japan/deploy-mon/blob/main/img/sample_image.png)