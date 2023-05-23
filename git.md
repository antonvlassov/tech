# Manter Repositórios

## Novo Repo 

```
criar repo no Git (http://<repo>.git)
git init
git remote add origin http://<repo>.git

git add README.md
git commit -m 'create repo - init commit'
git push origin master  (solicitado login e senha)
git push --set-upstream origin master (em alguns casos)
git branch develop
git checkout develop
git add .
git commit -m 'first version'
git push origin develop

```

# Configurar o git

## Global Confs

```
git config --global user.email "anton.vlassov.@email.com"
git config --global user.name "Anton Vlassov"
git config --global credential.helper store 
git config --global credential.https://github.com.useHttpPath true
git config --global core.filemode false
git config --global core.editor "vim"
```
Verificar as credenciais salvas:
`vim ~/.git-credentials`

Listas as confs:
`git config --list`


## Chave SSH

Fazer para install novo ou gerado

`ssh-keygen -t rsa -b 4096 -C "anton.vlassov.@email.com"`

Chaves serão geradas em:

```
Your identification has been saved in /home/anton/.ssh/id_rsa
Your public key has been saved in /home/anton/.ssh/id_rsa.pub

```
`ssh-add ~/.ssh/id_rsa`

Importar chave pública na conta do github:

`cat ~/.ssh/id_rsa.pub`

Navegar em Profile => Settings => SSH and GPG Keys => SSH Keys => Add New

Testar:

`ssh -T git@github.com`

Deve receber mensagem: _You've successfully authenticated, but GitHub does not provide shell access._


## Associar ao repo existente

```
git clone http://<<git>>.git

```

## Alterar remote

```
git remote set-url origin http://<<git>>.git
```

## Compare
`git diff branch1..branch2` - compara o HEAD dos dois branches
`git log branch1..branch2` - compara hist de commit entre dois branches

compara branch local com o mesmo branch remoto (repo)
```
git fetch
git diff master origin/master

```
## Merge

Realizar merge controlado das últimos updates do remote para local
```
git status
git fetch 
git merge
git status
```

Realizar merge do **"develop"** para **"master"**:
1. realizar checkout do branch destino (master): `git checkout master` (ambos os branches de origem e destino devem estar presentes local)
2. validar que o master esta sync com local: `git fetch / git merge / git status`
3. realizar merge: `git merge develop --squash`. 
    * `--squash` faz com que um único commit de merge seja criado, em vez de preservar a sequencia de commit inicial (isso é, merge nao faz commit no branch destino) 
    * `--allow-unrelated-histories` corrige o erro de "refused to merge unrelated stories"


## Utilidades
`git reset --hard origin/master` - reverte para situação atual do master APAGANDO as alterações locais (sem recuperação)

`git remote -v` - ver os remotes associados ao repositorio

`git config --list` - ver as confiiguracoes aplicadas ao repositório

`git branch -a` - lista branches locais e remotos

## atualizar .gitignote  
### metodo radical - full cleanup commit

commitar / push das alterações relevantes no código antes

```
git rm -r --cached .
git add .
git commit -m ".gitignore is now working"
```

## Criar um Branch Histórico a partir do Master
```
git checkout master
git pull (realizar merge)
git branch historic-1-2-0
git checkout historic-1-2-0
git merge master --squash
git push --set-upstream origin historic-1-2-0

```

# Geral

## Correção de Problemas de Certificado
# Correção de Problemas de Certificado

Erro: fatal: unable to access 'https://github.com/hyperledger/fabric-chaincode-go/': server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none

openssl s_client -showcerts -servername github.com -connect github.com:443 </dev/null 2>/dev/null | sed -n -e '/BEGIN\ CERTIFICATE/,/END\ CERTIFICATE/ p'  > github-com.pem
cat github-com.pem | sudo tee -a /etc/ssl/certs/ca-certificates.crt

openssl s_client -showcerts -servername google.golang.org -connect google.golang.org:443 </dev/null 2>/dev/null | sed -n -e '/BEGIN\ CERTIFICATE/,/END\ CERTIFICATE/ p'  > google-golang-org.pem
cat google-golang-org.pem | sudo tee -a /etc/ssl/certs/ca-certificates.crt

# Git Flow

`git flow init`  - inicializar git flow

## Feature

- criar nova feature:
`git flow feature start new-feature` 

- obter uma feature existente:
`git flow feature pull new-feature`

- comitar as alterações
```
git add .
git commit –m ‘comentarios’
```

- publicar feature:
`git flow feature publish new-feature`

- finalizar feature:
`git flow feature finish new-feature`

- sync o develop:
`git push origin develop`

## Release
- criar nova release:
`git flow release start new-release`

- acompanhar release existente:
`git flow release track my-new-release`

- comitar as alterações (last minutes fixes)
```
git add .
git commit –m ‘last-minute-release-bugfix’
```
- publicar release:
`git flow release publish new-release`

- finalizar release:
`git flow release finish new-release`

- sync repos
```
git push origin develop
git checkout master
git push origin master
```
## HotFix
- criar hotfix:
`git flow hotfix start emergency-hotfix`

- comitar as alterações (hot-fixes)
```
git add .
git commit –m ‘emergency-hotfix’
```

- publicar hotfix:
`git flow hotfix publish emergency-hotfix`

- finalizar hotfix:
`git flow hotfix finish emergency-hotfix`

- sync repos:
```
git push origin develop
git checkout master
git push origin maste

```


