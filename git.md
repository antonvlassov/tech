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



## Associar ao repo existente

```
echo "# docker-react" >> README.md
git init
git remote add origin https://github.com/antonvlassov/docker-react.git
git add README.md
git commit -m "first commit"
git push -u origin master
git remote set-url origin https://github.com/antonvlassov/docker-react.git

```

## Alterar remote

```
git remote set-url origin http://<<git>>.git
```