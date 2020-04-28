# Docker

http://files.cod3r.com.br/apostila-docker.pdf
https://github.com/cod3rcursos/curso-docker

#### 1.4 - O que são imagens Docker ?

https://hub.docker.com

Layers (camadas) também conhecidas como imagens intermediarias, são as mudanças realizadas na imagem original. Ex: imagem original container ubuntu. Criou um novo arquivo o SO add uma layer, etc... As layers são somente leitura. A junção de todas as layers formam a imagem. Após o container ser iniciado uma nova layer é criada. Esta é a unica q nao é read only e que pode ser alterada

Analogia: Imagem = classe, container = instacia do obj criado

---------------------------------------------------------------------------------------------------------------------------

#### 1.5. Arquitetura

Registry: Repositoriio de imagens (nuvem). Pode ser instalado localmente na infra da empresa tb.

Docker Daemon = Docker server = Docker engine: Instalado na maquina host. Comunica com registry (nuvem) para baixar as imagens

Cliente: CLI ou API ou Kitematic

---------------------------------------------------------------------------------------------------------------------------

#### Instalação - Visão geral 

No Mac, quando instala o Docker ele cria uma VM Linux. Somente o client fica no host MacOS de fato. (Windows funciona da mesma forma. Somente Windows 10 tem um esquema diferente, Windows subsystem for Linux)

Versão mais recente do Mac install Docker. Comandos são executados direto no terminal do MAC. Versões mais antigas intalar Docker Toolbox

https://hub.docker.com/editions/community/docker-ce-desktop-mac

---------------------------------------------------------------------------------------------------------------------------

#### 3.1. Introdução ao Docker Client
```sh
$ docker container run {nome-imagem}
$ docker container run hello-world
```
Comando acima verificou que a imagem não existia localmente, baixou a img do dockerhug e executou a imagem. 

Concatenação dos comando: docker image pull, docker container create, docker container start, docker container exec 

---------------------------------------------------------------------------------------------------------------------------

#### 3.4. Modo interativo

Executa o container, roda o comando bash. Cada vez q executar esse comando um novo processo é criado (container)
```sh
$ docker container run debian bash --version
```
Mesmos comandos unix podem ser executados nos containers Docker

Maquina host:
```sh
$ ps
```
Containers:
```sh
$ docker container ps -a
```
Se excutar com o comando abaixo após execução o container é removido.
```sh
$ docker container run --rm debian bash --version
```
---------------------------------------------------------------------------------------------------------------------------

#### 4.2. Diferenças entre container e imagem

Analogia: Imagem = classe, container = instacia do obj criado
```sh
$ docker container --help
$ docker image --help
$ docker volume --help
```
---------------------------------------------------------------------------------------------------------------------------

#### 4.9. Instruções com configuração para execução do container

No Dockerfile, colocar os comandos que não serão alterados com frequencia no inicio e os que mudam com mais frequencia no final. Isso faz com que menos layers sejam criadas no build, quando existe a mudança na imagem, recompilando menos layers, pois o compilador inicia no ponto onde ele identifica uma mudança na imagem em relação a imagem anterior.

Por convenção quando não informada a versão o latest é utilizado. Cuidado com essa abordagem pois se for criada uma versão mais recente definindo a versão, o latest ficará desatualizado caso não seja apontado para a versão mais atual.

Executa o container principal
```sh
$ docker container run -it -v $(pwd):/app -p 80:8000 --name python-server ex-build-dev
```
Executa container secundario que ira acessar o volume do container acima
```sh
$ docker container run -it --volumes-from=python-server debian cat /log/http-server.log
```
---------------------------------------------------------------------------------------------------------------------------

#### DockerHub.com
```sh
docker login
```
Username: <Docker Hub/Store Username>
Password: <Docker Hub/Store Password>
Login successful.

Push para DockerHub. Criar uma nova tag da imagem antes
```sh
$ docker image build -t movie-info-service-img ./movie-info-service
$ docker image tag movie-info-service-img dhiegoduarte/movie-info-servloginice-img:0.0.2
$ docker image push dhiegoduarte/movie-info-service-img:0.0.1

$ docker image build -t movie-catalog-service-img ./movie-catalog-service
$ docker image tag movie-catalog-service-img dhiegoduarte/movie-catalog-service-img:0.0.2
$ docker image push dhiegoduarte/movie-catalog-service-img:0.0.1

$ docker image build -t discovery-server-img ./discovery-server
$ docker image tag discovery-server-img dhiegoduarte/discovery-server-img:0.0.2
$ docker image push dhiegoduarte/discovery-server-img:0.0.1

$ docker image build -t ratings-data-service-img ./ratings-data-service
$ docker image tag ratings-data-service-img dhiegoduarte/ratings-data-service-img:0.0.2
$ docker image push dhiegoduarte/ratings-data-service-img:0.0.2
```
---------------------------------------------------------------------------------------------------------------------------

#### Limpar ambiente host

The docker system prune command will remove all stopped containers, all dangling images, and all unused networks:
```sh
$ docker system prune
```
To additionally remove any stopped containers and all unused images (not just dangling images), add the -a flag to the command:
```sh
$ docker system prune -a
$ docker system prune -a -f
```
Remove todas as imagens
```sh
$ docker images rmi $(docker images -a -q)
```
Remove container
```sh
$ docker container rm nome-container
```
---------------------------------------------------------------------------------------------------------------------------

#### Redes (bridge, host e none. Por padrão a utilizada é a bridge 172.17.0.0/16):

Listar as redes existentes
```sh
$ docker network ls
```
Criar nova rede (172.18.0.0/16)
```sh
$ docker network create --driver bridge rede_nova
```
Definir container para utilizar a rede nova
```sh
$ docker container run -d --name container3 --net rede_nova debian
```
Associar a rede bridge ao container configurado com a rede nova. Vai ter acesso as duas redes (vai ter duas interfaces de rede)
```sh
$ docker network connect bridge container3
$ docker network disconnect bridge container3
```
Definir container sem acesso a rede
```sh
$ docker container run -d --net none debian
```
---------------------------------------------------------------------------------------------------------------------------

#### Comandos gerais
```sh
$ docker container --help
$ docker imagem --help
$ docker volume --help
$ docker network --help

$ docker container run {nome-imagem}
$ docker container start -i ratings-data-service-ctnr
$ docker container stop discovery-server-ctnr
```
Executa o container, roda o comando bash. Cada vez q executar esse comando um novo processo é criado (container)
```sh
$ docker container run debian bash --version
```
Lista todos os containers:
```sh
$ docker container ls -a
```
Se excutar com o comando abaixo após execução o container é removido.
```sh
$ docker container run --rm debian bash --version
```

Compartilhamento de volume
Executa o container principal que em seu dockerfile expoe o volume /log
```sh
$ docker container run -it -v $(pwd):/app -p 80:8000 --name python-server ex-build-dev
```
Executa container secundario que ira acessar o volume do container acima
```sh
$ docker container run -it --volumes-from=python-server debian cat /log/http-server.log
```
To open an interactive console to inspect/work on the container, run (container is up already)
```sh
$ docker container exec -it ratings-data-service-ctnr bash
```
Verificar as configs do container
```sh
$ docker container inspect ratings-data-service-ctnr
```
---------------------------------------------------------------------------------------------------------------------------

#### Docker compose

Cria e inicia todos os serviços em modo daemon (parametro -d plano de fundo sem ficar com o terminal travado na execuçã do processo)
```sh
$ docker-compose up -d
```
Cria e inicia somente o serviço (container) db. Se este tiver dependencias com outros, entao, os demais serviços que este depende serão iniciados
```sh
$ docker-compose up -d db
```
Similar ao docker ps, mas se limitando aos serviços indicados no docker-compose.yml
```sh
$ docker-compose ps
```
Similar ao docker exec, mas utilizando como referência o nome do serviço db
```sh
$ docker-compose exec db
```
Para todos os serviços e remove os containers (Stop and remove containers, networks, images, and volumes)
```sh
$ docker-compose down
```
Com docker-compose cada serviço (container) definido no composer tem o nome (o que esta definido no compose) como host, ou seja, os demais containers podem acessar este atraves deste nome. Todos os serviços (containers) criados no compose se enxergam entre si desde que não tenha sido criadas redes segregadas (definidas nos servicos no proprio compose).
```sh
$ docker-compose logs -f -t
```
Escalando container worker definido no compose
```sh
$ docker-compose up -d --scale worker=3
```
Inicia todos os serviços
```sh
$ docker-compose start
```
Para todos os serviços
```sh
$ docker-compose stop
```
Boa prática utilizar docker-compose mesmo para criação de um unico container pois ao executar ele já remove as imagens intermediarias criadas (liberando espaço em disco) assim como os parametros que seriam passados no docker container run são definidos explicitamente no compose (mapeamento de porta e volumes) o que traz melhor governança da infraestrutura criada

---------------------------------------------------------------------------------------------------------------------------

Docker Root Dir (para saber o local executar docker info --format '{‌{.DockerRootDir}}')

Ter um repositório git por microserviço para poder a cada commit executar um pipeline especifíco de CI (Jenkins)

---------------------------------------------------------------------------------------------------------------------------

No /etc/hosts do container é criado um mapeamento com ip e host com o id do container!!! Esse mesmo host é adicionado ao Eureka server para comunicar com o MS publicado neste container.

---------------------------------------------------------------------------------------------------------------------------

Executar container passando argumento que substitui o definido no CMD
docker container run -it -p 8081:8081 --name movie-catalog-service-ctnr movie-catalog-service-img movie-catalog-service-0.0.108-SNAPSHOT.jar

---------------------------------------------------------------------------------------------------------------------------

Adicionando proxy a imagem docker. Proxy será usado para quando for baixar da internet para atualizar os pacotes do linux por exemplo.

docker image build --build-arg http_proxy=http://proxy.claro.com.br:3128 -t movie-catalog-service-img ./movie-catalog-service

---------------------------------------------------------------------------------------------------------------------------

#### Validar imagem 

```sh
$ docker run --rm -i hadolint/hadolint < ./movie-catalog-service/Dockerfile
```
---------------------------------------------------------------------------------------------------------------------------

#### Reiniciar Docker 

```sh
$ sudo systemctl daemon-reload && sudo systemctl restart docker && sudo docker info
```
---------------------------------------------------------------------------------------------------------------------------