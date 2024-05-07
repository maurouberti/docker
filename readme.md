![Static Badge](https://img.shields.io/badge/nginx-1.25.5--alpine-brightgreen?logo=nginx)
![Static Badge](https://img.shields.io/badge/php-8.3--fpm--alpine-%238892BF)

Um contêiner **docker** opera com um servidor web **nginx**, configurado para se comunicar internamente com outro contêiner que executa o **php-fpm**.



# Comandos

Alguns comandos **docker** foram usados para iniciar esses contêineres.



### Criar rede

```
docker network create minha_rede
```

> listar redes: `docker network ls`

```
docker network connect minha_rede nginx
docker network connect minha_rede php_fpm
```

> informações da rede: `docker network inspect minha_rede`



### Remover rede

```
docker network rm minha_rede
```



### Criar volume

```
docker volume create meu_volume
```

> listar volumes: `docker volume ls`



### Remover volume

```
docker volume rm meu_volume
```



### Construir imagem

```
docker build -t maurouberti/php-fpm:1.0.0 ./php-fpm
docker build -t maurouberti/nginx:1.0.0 ./nginx
```

> Colocar versão para não criar nova imagem



### Remover **todas** imagem

```
docker rmi -f $(docker images -aq)
```

> docker images -aq: lista de **todas** imagens

> remover somente as imagens não utilizadas: `docker image prune -f`



### Criar container

```
docker container run --name php_fpm -d --network minha_rede -v meu_volume:/var/www maurouberti/php-fpm:1.0.0
docker container run --name nginx -d -p 80:80 --network minha_rede --link php_fpm:php_fpm -v meu_volume:/var/www maurouberti/nginx:1.0.0
```

> nome do container: `--name nginx`

> executado em segundo plano: `-d`

> porta externa: `-p 80:80`

> seleciona a rede: `--network minha_rede`

> dependencia de outro container: `--link php_fpm:php_fpm`

> seleciona o volume: `-v meu_volume:/var/www`

> imagem: `maurouberti/nginx:1.0.0`



### Iniciar container

```
# modo interativo
docker start -i php_fpm
docker start -i nginx

# segundo plano
docker start php_fpm
docker start nginx
```

> Entrar no container: `docker exec -it php_fpm /bin/sh`

> Log: `docker logs -f nginx`


### Remover **todos** containers

```
docker rm -f $(docker ps -aq)
```

> docker ps -aq: lista de **todos** containers

> remover somente os containers não utilizados: `docker container prune -f`



### Enviar para dockerhub

```
docker login

docker tag maurouberti/php-fpm:1.0.0 ubertimauro/php-fpm:1.0.0
docker push ubertimauro/php-fpm:1.0.0

docker tag maurouberti/nginx:1.0.0 ubertimauro/nginx:1.0.0
docker push ubertimauro/nginx:1.0.0
```

> docker tag {local} {dockerhub}
