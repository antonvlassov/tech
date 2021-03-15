# Listagem de Dependências

## Maven 2

Exportar toda a lista de dependências (pom.xml)

```
mvn dependency:tree -Dverbose
mvn dependency:tree -Dverbose > project_dependencies.txt

```

Verifica onde existe depend. específica:
`mvn dependency:tree -Dverbose -Dincludes=commons-collections`

## Maven 3
mvn org.apache.maven.plugins:maven-dependency-plugin:2.10:tree -Dverbose=true


