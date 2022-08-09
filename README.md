# docker
git clone https://github.com/AlariCode/docker-demo-4.git
# Использование volumes
cd docker-demo-4/
docker build -t demo4:latest .
docker volume create demo
docker volume ls
docker volume inspect demo
sudo ls /var/lib/docker/volumes/demo/_data
docker run --name volume-1 -d -v demo:/opt/app/data demo4:latest
sudo ls /var/lib/docker/volumes/demo/_data
docker rm -f volume-1
docker run --name volume-1 -d -v demo:/opt/app/data -p 3000:3000 demo4:latest
curl "127.0.0.1:3000/set?id=123"
curl "127.0.0.1:3000/get"
docker run --name volume-2 -d -v demo:/opt/app/data -p 3001:3000 demo4:latest
curl "127.0.0.1:3001/get"
docker volume rm demo
docker ps
docker rm -f volume-1
docker rm -f volume-2
docker volume rm demo
docker volume ls
# driver VOLUME 
sudo nano Dockerfile
...
VOLUME ["/opt/app/data"]
...
docker build -t demo4:latest .
docker image inspect demo4
docker volume ls
docker run --name volume-1 -d -p 3000:3000 demo4
docker volume ls
curl "127.0.0.1:3000/set?id=123"
docker rm -f volume-1
docker volume create demo
docker volume ls
docker run --name volume-1 -d -p 3000:3000 -v demo:/opt/app/data demo4

# driver BIND MOUNTS
docker run --name volume-1 -d -p 3000:3000 -v /home/docker/data:/opt/app/data demo4
docker ps
cd ~
ls data/
curl "127.0.0.1:3000/set?id=123"
ls data/
cat data/req
curl "127.0.0.1:3000/get"

# tmpfs
docker run --name volume-1 -d -p 3000:3000 --tmpfs /opt/app/data demo4
docker ps
curl "127.0.0.1:3000/set?id=123"
curl "127.0.0.1:3000/get"
docker stop volume-1
docker start volume-1
curl "127.0.0.1:3000/get"

docker run --name volume-2 -d -p 3001:3000 --mount type=tmpfs,destination=/opt/app/data demo4

# копирование файлов
docker run --name volume-3 -d -p 3000:3000 demo4
curl "127.0.0.1:3000/get"
docker cp /home/docker/projects/docker/docker-demo-4/data/req volume-3:/opt/app/data
curl "127.0.0.1:3000/get"
docker cp volume-3:/opt/app/data /home/docker/projects/docker/docker-demo-4/test
cat test/req
docker cp volume-3:/opt/app/data/req /home/docker/projects/docker/docker-demo-4/test2/req
cat test2/req
