# vg-devops1_microservices
vg-devops1_microservices repository
Task No: 14

Docker images saved into docker-monolith/docker-1.log

Task No: 15
1) docker-232319 created in GCP
2) gcloud init was launched to select default region 
3) command gcloud auth application-default login, default project and region were confirmed, gcp credentials for futher terminal access were downloaded
4) docker-machine was installed locally, on Linux Mint 18.02 (same as for Ubuntu 16.04)
5) docker-machine launched an instance in GCP, docker-232319 project area, with Ubuntu 16.04 with a docker client installed
6) docker-machine ls - showed working instance with the docker version on it
7) command eval $(docker-machine env docker-host) sets default environment for docker as "docker-host" instance (remote)
8) 4 files were created in /docker_monolith folder:  db_config  Dockerfile  mongod.conf  start.sh as provided in the task
9) command "docker build -t reddit:latest . " creates a remote container on the "docker-host". Dot is used as a reference to Dockerfile
10) command "docker run --name reddit -d --network=host reddit:latest" starts the created container on this remote host
11) command "docker-machine ls" shows the following output:
NAME          ACTIVE   DRIVER   STATE     URL                       SWARM   DOCKER     ERRORS
docker-host   -        google   Running   tcp://34.76.97.236:9292           v18.09.2 

12) Using google SDK command: gcloud compute firewall-rules create reddit-app \
 --allow tcp:9292 \
 --target-tags=docker-machine \
 --description="Allow PUMA connections" \
 --direction=INGRESS 

 we added firewall rule to ingress tcp port access 9292. The link 34.76.97.236:9292 has become accessible with puma server running via the container

 13) Registered on Docker Hub as vgdevops1
 14) Pushed the created container image to docker hub into the following repo: https://hub.docker.com/r/vgdevops1/otus-reddit, using commands as

    docker tag reddit:latest vgdevops1/otus-reddit:1.0
    docker push vgdevops1/otus-reddit:1.0

 15) switched to local docker using command: eval $(docker-machine env -u)

 16) launched container from the remote docker hub/image using command:
 docker run --name reddit2 -d -p 9292:9292 vgdevops1/otus-reddit:1.0

 17) Container starts locally as shown by the output from command "docker ps -a"
 CONTAINER ID        IMAGE                       COMMAND             CREATED              STATUS              PORTS                    NAMES
4c867148b6a3        vgdevops1/otus-reddit:1.0   "/start.sh"         About a minute ago   Up About a minute   0.0.0.0:9292->9292/tcp   reddit2
 
 18 ) Created bash instance inside the reddit container via command "docker exec -it reddit2 bash", logged into the container and completed the remaining operations, such as listing processes, killing the parent process in the container, forcing it to close etc