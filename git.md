# Manter Repositórios

## Novo Repo 

```
criar repo no Git (http://<repo>.git)
git init
git remote add origin http://<repo>.git
git add README.md
git commit -m 'create repo - init commit'
git push origin master  (solicitado login e senha)
git branch develop
git checkout develop
git add .
git commit -m 'first version'
git push origin develop

```

# Configurar o git
```
git config --global user.email "anton.vlassov.@email.com"
git config --global user.name "Anton Vlassov"
git config --global credential.helper store 
git config --global credential.github.com.useHttpPath true
```
Verificar as credenciais salvas:
`vim ~/.git-credentials`

## Associar ao repo existente

```
git clone http://<<git>>.git

```

## Alterar remote

```
git remote set-url origin http://<<git>>.git
```