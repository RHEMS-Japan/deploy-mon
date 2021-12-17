# Requirement

- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [Docker Compose](https://docs.docker.com/compose/install/)

# Usage

## Run containers using docker-compose

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


## Login to the dashbord container

- sample command
```
docker exec -it 463ecaa1c2ee  sh
```


## Install the curl command

- command
```
apk add curl
```







## Send a POST request to api-update-info

- command
```
curl -X POST http://check-mon:5000/api-update-info
```

- sample result
```
/app # curl -X POST http://check-mon:5000/api-update-info
[]
/app #
```


## Send a GET request to api-check-version


- command
```
curl http://check-mon:5000/api-check-version
```

- sample result
```
/app # curl http://check-mon:5000/api-check-version
{'insert get_check_version'}
/app #
```


## Send a GET request to api-baseinfo

- command
```
curl http://check-mon:5000/api-baseinfo
```

- sample result
```
/app # curl http://check-mon:5000/api-baseinfo
{"all_apps":0,"all_host":0,"last_update":"2021-12-16 18:55:39.385000"}
/app #
```

## Access the following URL in your browser

- URL
```
http://0.0.0.0:3000/
```

- sample image

![sample image](https://github.com/RHEMS-Japan/deploy-mon/blob/main/img/sample_image.png)
