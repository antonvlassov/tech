# Install Docker

# prereq
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

# add repo
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(. /etc/os-release; echo "$UBUNTU_CODENAME") stable"

sudo apt-get update

# install docker
sudo apt-get install docker-ce docker-ce-cli containerd.io

sudo usermod -aG docker $USER
su - $USER


# init docker
sudo systemctl start docker
(https://docs.docker.com/install/linux/linux-postinstall/#configure-docker-to-start-on-boot)
 sudo systemctl stop docker

# docker compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version

# Comandos Basicos

## Executar Imagem

```
- override
docker run busybox echo hi there
docker run busybox ls  -folders que existem dentro do container 
 -- porem esses comandos devem existir previamente na imagem de FS

docker ps - todos os containers em execução, pega id para outros comandos
docker ps --all - historico de todos os conatiners
docker system prune -limpa o sistema, inclusive os cache das imagens, limpa o espaço ocupado

docker run - = docker create + docker start
docker run -d - executa o container em background, sem jogar output no shell

docker create hello-world
docker start -a CONTAINER-ID (-a faz com que o CLI receba o output do conatiner no terminal)

Exited (0) container

docker start -a d05c33003c0d (id do container do docker ps, não pode trocar docker command)

docker start ID (inicia sem o output para terminal)
docker logs ID (lista tudo que aconteceu no output - mesma coisa que docker start -a)

docker stop ID  - via SIGTERM - respeita o tempo do processo para terminar, espera 10 segundos, e manda docker kill caso não tenha resposta nesse tempo 
docker kill ID - via SIGKILL - mata imediatamente

docker exec -it ID COMMAND (-i associa terminal com stdin do container, -t formatação dos comandos internos enviados de volta para terminal) command para o container em execução com ID informado; permite que após execução o comando o container tenha a capacidade de receber inputs via linha de comando)
dcoker exec - PARA CONTAINER JÁ NO AR

docker exec -it bc21714d0259 redis-cli

docker exec -it ID sh - obtem o shell 'dentro' do conatiner para executar comandos (CTRL+D para sair) 'sh' é um commando processer assim como bash

docker exec -it ID bash - executa o processador bash (exit para sair)

docker run -it busybox sh -novo container da iagem busybox iniciando com 'sh' como processador de comando (não é recomendado)

ID do conatainer - pode ser referenciado pegando qualquer quantidade dos primeiros caracteres desde que sejam únicos

CONTAINER ID e IMAGE ID são diferentes


```

## Gestao de Imagens

```
docker build . (vai retornar o ID)
docker build -t dockerid/project_ou_repo:version - (adiciona tag a imagem)

```
## Logs

```
- verificar logs
docker logs 8197fd5e6335

- navegar no workdir
docker exec -it 8197fd5e6335 sh


```

## Docker Compose
```
docker-compose up
docker-compose up -d (em background)
docker-compose up --build (força rebuild das imagens)
docker-compose down
docker-compose ps - listagem, precisa ter docker-compose.yml, pois ele verifica status somente dos containers declarados

```




# Troubleshooting

Docker - falta de espaço
```
df -hi
docker volume ls -q -f dangling=true

docker volume rm `docker volume ls -q -f dangling=true`

```