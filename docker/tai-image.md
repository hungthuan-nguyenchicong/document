> docker pull nginx:alpine

> docker images

> docker run -d --name nginx-vui-ve -p 8080:80 nginx:alpine

> curl localhost:8080

> docker ps

> docker stop nginx-vui-ve

> docker rm -f nginx-vui-ve

> docker ps -a

> docker rm -f nginx-custom

> docker rm -f $(docker ps -aq)

> docker system prune -a

```docker
docker run -d \
--name nginx-custom \
-p 8080:80 \
-v $(pwd)/html:/usr/share/nginx/html:ro \
nginx:alpine

```

```docker
docker run -d \
--name nginx-custom \
-p 8080:80 \
-v $(pwd)/index.html:/usr/share/nginx/html/index.html:ro \
nginx:alpine

```

> docker run -d --name nginx-custom -p 8080:80 -v $(pwd)/index.html:/usr/share/nginx/html/index.html:ro nginx:alpine
