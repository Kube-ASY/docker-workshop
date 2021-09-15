# Docker Grundlagen

```
#01
docker run hello-world
```
```
#02
docker run alpine echo hello world
```
```
#03
docker run -it debian bash
```
```
#04
docker run -it -p 90:80 debian bash
```
```
#05
apt-get update && apt-get install -y apache2
```
```
#06
apache2ctl -DFOREGROUND
```
```
#07
docker run --name my-apache -d -p 8080:80 httpd:2.4
```
```
#08
docker exec -it my-apache bash
```
```
#09
docker run --name vol-apache -v ~/www-data/:/usr/local/apache2/htdocs/ -d -p 8090:80 httpd:2.4
```
```
#10
docker exec -it vol-apache bash
```
```
#Optional:
docker volume create html
docker run --name vol-nginx -d -p 8081:80 -v html:/usr/share/nginx/html -d nginx
```
```
"docker inspect"
docker inspect --format='{{.Config.Image}}' vol-nginx
docker inspect --format='{{ .NetworkSettings.IPAddress }}' vol-nginx
```

# Dockerfile
```
#11
FROM debian
RUN apt-get update && apt-get install apache2 -y && apt-get clean
CMD ["apache2ctl", "-DFOREGROUND"]
```
```
#12
docker build -t myimage .
docker run -d -p 90:80 --name myapp myimage
```

# Docker-Compose
#Wordpress
```yaml
version: '3.1'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8083:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wp
      WORDPRESS_DB_PASSWORD: geheim
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wp
      MYSQL_PASSWORD: geheim
      MYSQL_RANDOM_ROOT_PASSWORD: geheim
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```

#Nodered
```yaml
version: "3.7"

services:
  node-red:
    image: nodered/node-red:latest
    environment:
      - TZ=Europe/Amsterdam
    ports:
      - "1880:1880"
    networks:
      - node-red-net
    volumes:
      - node-red-data:/data

volumes:
  node-red-data:

networks:
  node-red-net:
```

# Docker in der Praxis
#Portainer
```
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```
