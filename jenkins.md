# Jenkins


## Multibranch pipeline libs

projeto => configure

tab "Pipeline Libraries", Add

- **Name:** nome da lib a ser referenciada no JenkinsFile, por exemplo _SharedLibTest_

- **Retrieval method:** Legacy SCM

- **Source Code Management:**
    - selecionar "Gir"
    - RepositoryUrl : adicionar Url do Git Repo onde estão armazenadas as shared libs
    - Credentials : adicionar credential store de autenticação Jenkins => Git Repo
    - Branch Specifier : adicionar a branch da qual os shared libs serão obtidos (ex.: _v2-test_)

Após essas alterações, configurar o JenkinsFile, apontando para Name da library + a branch utilizada:
`@Library('SharedLibTest@v2-test')` 


## Add JDK

Manage Jenkins => Global Tool Configirarions

Na página, encontrar "JDK" => "JDK Installations"

Configurar os installers dos JDK conforme necessidade.
A identificador "Name" em cada "JDK" será utilizado para refrenciar o JDK dentro JenkinsFile
ATENÇÃO: campo "Label" - não é qualquer "label" informativo
- esse campo restringe a instalação desse JDK somente aos agentes que tenham o mesmo Label
- se os labels não forem compatíveis, da erro dutante a execução da pipeline (a instalação é feita on-demand, na primeira execução):  `Installer "Extract .zip/.tar.gz" cannot be used to install "adoptOpenJdk11" on the node "jenkins-slave-jnlp"`
- se não há necessidade desse tipo de restrição, DEIXAR O LABEL EM BRANCO

