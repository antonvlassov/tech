## Comandos básicos
`$ cd ~`   => entra no home /home/nome_do_usuario do usuario logado

`$ rm -rf tempo/*`  => remove conteudo do diretorio

`$ rm -rf tempo`  => remove todo o diretorio

`$ rm *` => remove conreudo do dirtorio atual

`$ tar -zcvf archive-name.tar.gz directory-name` => descompacta arquivo em um diretório

`$ find tempo/*` => lista todos os artefatos com caminho completo dentro do diretorio

`$ chmod +x OAG-11.1.2.4.0-linux-x64-installer.run` => transforma arquivo em executável

`$ touch "foo.backup.$(date +%Y%m%d_%H%M%S)" `=> cria arquivo com timestamp no nome

`$ sudo -i` => muda para usuario root

grep

`$ grep --include=\*.props -rnw '/opt/middleware/OAG-11.1.2.4.0/apigateway' -e "env."`

`$ grep --include=\*.{c,h} -rnw '/path/to/somewhere/' -e "pattern"`

`$ grep --exclude=*.o -rnw '/path/to/somewhere/' -e "pattern"`

`$ grep --exclude-dir={dir1,dir2,*.dst} -rnw '/path/to/somewhere/' -e "pattern"`

Links simbólicos:

`ln -s /usr/local/bin/wkhtmltopdf wkhtmltopdf`

`ls -al`

netstat

`$ netstat -lnpt`

`$ netstat -lnp`

`$ netstat -rn`
`$ nc localhost 12345` - verifica se socket está aberto

loops
```
#!/bin/bash
for i in 1 2 3 4 5
do
   echo "Welcome $i times"
done
```
```
for f in *.temp
do
   mv $f $(echo "$f" | cut -f 1 -d '.').yaml
done
```

verificar capacidade da máquia

`$ cat /proc/cpuinfo` <- cpu

`$ free -m`           <- memoria  

teste rapido de conectividade:

```
sudo apt-get install mini-httpd
sudo service mini-httpd start
sudo mini_httpd -p 80
wget localhost
```

## Shell Script
```
if [ $# -eq 0 ]
   then
        echo 'invalid number of arguments'
        exit 1
fi

echo $1

if [ "$1" == "Core" ]
    then
        echo 'Installing Core Module'
fi

if [ "$1" == "Analytics" ]
    then
        echo 'Installing Analytics Module'
fi

#!/bin/bash
count=99
if [ $count -eq 100 ]
then
  echo "Count is 100"
elif [ $count -gt 100 ]
then
  echo "Count is greater than 100"
else
  echo "Count is less than 100"
fi

If [ conditional expression1 ]
then
	statement1
	statement2
	.
else
	if [ conditional expression2 ]
	then
		statement3
		.
	fi
fi

```

Criar um .sh para subir varios services de um tipo (sobe todos os serviços do hadoop)
```
echo 'for x in `cd /etc/init.d ; ls hadoop-hdfs-*` ; do sudo service $x start ; done' >> start_hadoop_services.sh
chmod +x start_hadoop_services.sh
./start_hadoop_services.sh
```



# Utilização com VM

Conexões:
```
ssh -p 2222 osboxes@localhost
scp -P 2222 -v flume-sources-1.0-SNAPSHOT.jar osboxes@localhost:
```

# CentOS

Desabilitar firewall:
```
sudo systemctl disable firewalld
sudo systemctl stop firewalld
sudo systemctl status firewalld

```

Testar se é acessivel criando um web server no node:
```
sudo yum install epel-release  <= adiciona novos repositprios para yum buscar mais arquivos
sudo yum install nodejs


var http = require('http');
//create a server object:
http.createServer(function (req, res) {
  res.write('Hello World!'); //write a response
  res.end(); //end the response
}).listen(3000, function(){
 console.log("server start at port 3000"); //the server object listens on port 3000
});
```

Instalar Docker
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo docker run hello-world

-- add usuario ao grupo docker para rodar sem sudo
https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user
sudo usermod -aG docker $USER
-- realizar logout/login
docker version

docker run hello-world

```

Instalar Docker Composer
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version

```








