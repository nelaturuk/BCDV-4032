# Docker Session Commands

```
\\ Check the status of docker installation
systemctl status docker

\\ Show current running containers
docker ps

\\ Show containers that have exited as welll
docker ps -a 

\\ Run ubuntu container
docker run -itd ubuntu

\\ Stop container
docker stop <cid>

docker stop $(docker ps -aq)

docker rm $(docker ps -aq)

\\ start container
docker start <cid>

docker ps -a 

\\Remove a container
docker rm <cid>

\\ Remove all stopped containers
docker container prune

\\ List all images
docker images

\\ Rmeove image
docker rmi ubuntu

docker image rm hello-world

\\ Prune images
docker image prune

\\ List all images
docker image ls -a

docker container ls -a

\\ Pull an image 
docker pull ubuntu

\\ Run ubuntu image
docker run ubuntu
docker ps -a

\\ Interactive mode
docker run -it ubuntu

\\ Exec
docker exec -it <cid> bash

docker inspect <cid>

docker pull jenkins

docker pull jenkins:2.603

docker image inspect cd

```

# Docker Networking

```
docker network ls

docker network inspect <nid>

docker network create --driver=bridge --subnet=182.1.0.0/16 isolatedNetwork

docker network rm <nid>

docker run -itd centos

docker ps
docker run -itd --name=test1 --net=isolatedNetwork centos
docker exec -it <cid> bash

ping test1

docker network connect isolatedNetwork <cname>

docker exec -it <cid> bash

ping test1


```

# Docker Volumes

```
cd /var/lib/docker/volumes

docker volume create data_volume

sudo ls -lrt

docker run -itd -v data_volume:/www ubuntu

docker ps

docker exec -it <cid> bash

ls

cd www

echo "Blash balsh" > test.txt

ls

exit

cd data_volume

cd _data

ls

docker ps
docker stop eb
docker rm eb

ls


docker run -itd -v data_volume:/www ubuntu
```