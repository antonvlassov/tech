# Windows Dual Boot

Seguir as instruções em:
https://fossinsider.com/dual-boot-linux-mint-and-windows11/

**Dicas:**

1. F1 (BIOS) - em Security, desabilitar Secure Boot para que possa bootar do USB os sistemas operacionais não homologados (não-Microsoft). Nunca mais reabilitar.
2. F12 - permite entrar em one-time boot selection mode (isso é, só usa a opção naquele boot) e selecionar boot do USB

Calcular o espaço em disco - do espaço total disponível para Linux, subtrair 1024 MB para Boot Partition, 2 x RAM GB para Swap. Restante dividir em duas partições (`/` e `/home`).

Assim que executar o Linux do USB, **antes de instalar a versão**, verificar o funcionamento de todos os drivers relevantes, principalmente WIFI, Ethernet e outros periféricos. Se não funcionar na imagem, não vai funcionar após a instalação final. Resolver todos os problemas na imagem


# Linux Mint

## Upgrade
```
sudo apt update
sudo apt upgrade
```

## Basic Software Install
```
sudo apt install terminator
sudo apt install git
sudo apt install git-flow
sudo apt install vlc
sudo apt install curl
sudo apt install wget
sudo apt install make
sudo apt install vim
sudo apt install zsh
sudo apt install snapd
```

## TroubleShooting
Para bugs e travamentos do Cinnamon:
ALT + F2, depois digitar "r" e "enter" para recarregar o ambiente desktop 

## Short Cuts

Keyboard => system

ctrl+shift+l - lock
ctrl+shift+del - suspend

## Confs Uteis
ajuste de brilho - icone da bateria, ajustar


# Install & Configure Software

## Google Chrome
fazer download do deb de https://www.google.com/chrome/
https://linuxhint.com/install_google_chrome_linux_mint/

```
cd ~/Downloads/
sudo dpkg -i google-chrome-stable_current_amd64.deb

```

Criar 3 profiles - principais, Web3 (Com todas as Wallets) e Dev (Com Wallets apontando para redes de testes  )

## Brave Browser

```
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list

sudo apt update

sudo apt install brave-browser

```
## Visual Studio Code
Baixar os binários de https://code.visualstudio.com/docs/?dv=linux64_deb
```
cd ~/Downloads/
sudo dpkg -i code_1.42.1-1581432938_amd64.deb
```

Configurar o VScode de acordo com https://github.com/antonvlassov/tech/blob/master/vscode.md

Aumentar o máximo possível de arquivos do file watch para grandes projetos node (muitos node_modules) em workspaces


editar o conf e adicionar novo valor no final do arquivo:
```
sudo vim /etc/sysctl.conf
fs.inotify.max_user_watches = 524288
```
confirmar novo valor setado
```
cat /proc/sys/fs/inotify/max_user_watches
```

## SDK Man
curl -s "https://get.sdkman.io" | bash
source "/home/anton/.sdkman/bin/sdkman-init.sh"

Os SDK gerenciados pelo SdkMan sao instalados nesse folder
`/home/anton/.sdkman`

Adicionar seguinte linha ao final do `.zshrc`:

**#THIS MUST BE AT THE END OF THE FILE FOR SDKMAN TO WORK!!!**

`export SDKMAN_DIR="/home/anton/.sdkman" [[ -s "/home/anton/.sdkman/bin/sdkman-init.sh" ]] && source "/home/anton/.sdkman/bin/sdkman-init.sh"`

Instalar JDK8 open source (AdoptJDK):
`sdk install java 8.0.242.hs-adpt `

Observação: Java Default é instalado em:
`ls /usr/lib/jvm `. Esse path pode ser encontrado por meio do comando `readlink -f $(which java)`

Listar versões:
`sdk list java`

Instalar versões - obter o campo "identifier" da listagem:
`sdk install java <<identifier>>`

Verificar qual versão é defalt (SDK já configura o JAVA_HOME):
`sdk current java`

Configurar como default:
`sdk default java <<identifier>>`

User versão específica
`sdk use java <<identifier>>`


## Spring Tool Suite
baixar binários do https://spring.io/tools

```
mkdir software
cd ~/Downloads
tar -zcvf spring-tool-suite-4-4.5.1.RELEASE-e4.14.0-linux.gtk.x86_64.tar.gz
sudo mv sts-4.5.1.RELEASE ~/Software
```
**Criação do atalho:**

No desktop, com botão direito, "+ create new launcher here"

Fonecer seguintes informações:
* **Name:** Spring Tool Suite 4
* **Command:** /home/anton/software/sts-4.5.1.RELEASE/SpringToolSuite4
* **Comment:** Spring Tool Suite 4
* **Launch in Terminal:** FALSE  [ ]

### Configurar 
```
window:
  preferences:
        java:
            build path:
              source folder name : src/main/java 
              output folder name : target
            installed JRES:
              add:
                JRE Home: /home/anton/.sdkman/candidates/java/8.0.242.hs-adpt
            compiler:
              JDK Compliance:
                Compiler Compliance Level: 1.8    

```
Premissa que o SDK man e JDK apopriada ter sido instalado

### Instalar Plugins
help -> eclipse marketplace

procurar e instalar:
* Dockerfile Editor
* Yedit Yaml Editor

### Importar Projetos

1. import -> general -> existing projects into workspace
2. project -> properties -> java build path -> alterar de /bin para /target
3. project -> maven -> update project -> [x] update dependencies -> [x] force update of snapshots/releases

## Instalar Maven
`sudo apt install maven`


## Node JS

```
curl -sL https://deb.nodesource.com/setup_12.x | bash -
sudo apt install nodejs 
```
Preparar npm para global module install, sem a necessidade de SUDO
```
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'

```
Adicionar PATH no .zshrc e recarregar o profile
```
export PATH=~/.npm-global/bin:$PATH

```

instalar instalador simples de node versions, para nao depender do repo do Ubunto e poder especificar versão apropriada

```
sudo npm install n -g
sudo -E env "PATH=$PATH" n 12.12.0

```
usar para a a necessidade de instalar a ultima stable:
`sudo -E env "PATH=$PATH" n stable`


## Kubernetes
Para instalar **Helm, Kubectl e MicroK8S**, seguir as intruções em:
https://github.com/antonvlassov/tech/blob/master/kubernetes.md


## Ebook Reader
Foliant - obter e instalar .deb do https://github.com/johnfactotum/foliate/releases

## Screen Capture
Nativo do Ubuntu
PrtSc - screenshot do desktop
Alt + PrtSc - da janela
Shift + PrtSc - permite selecionar


# Shell

## Configs 

Configurar  ZSH como default shell :

```
zsh --version
whereis zsh
sudo usermod -s /usr/bin/zsh $(whoami)

```
Tornar o autocomplete case-insenstive
```
$ if [ ! -a ~/.inputrc ]; then echo '$include /etc/inputrc' > ~/.inputrc; fi
$ echo 'set completion-ignore-case On' >> ~/.inputrc
```

## Instalar OhMyZSH

Para instalar, executar os comandos abaixo:

```
chsh -s $(which zsh)
sh -c “$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
* Site oficial e código-fonte: https://github.com/ohmyzsh/ohmyzsh
* Localização dos plugins na máquina local: `cd ~/.oh-my-zsh/plugins`
* Lista de plugins disponíveis: https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins

## Instalar plugins adicionais:

```
sudo apt install zsh-theme-powerlevel9k
sudo apt install zsh-syntax-highlighting
sudo apt install zsh-autosuggestions

```
Adicionar conf para habilitar dentro do zsh
```
echo "source /usr/share/powerlevel9k/powerlevel9k.zsh-theme" >> ~/.zshrc
echo "source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh" >> ~/.zshrc
echo "source /usr/share/powerlevel9k/powerlevel9k.zsh-theme" >> ~/.zshrc

```
## Configurar Prompt

Prompt do powerlevel:

https://github.com/Powerlevel9k/powerlevel9k/wiki/Stylizing-Your-Prompt
https://github.com/Powerlevel9k/powerlevel9k#context


## Configurar ZSH / OhMyZSH
Abrir o arquivo de configuração do shell: `vim .zshrc`

1. Copiar o `$PATH` do `.bashrc` atual
2. Configurar o tema: `ZSH_THEME=agnoster`
3. Configurar seguintes plugins:
```
  plugins=(
    git
    git-flow
    docker
    docker-compose
    kubectl
    helm
    npm
    node 
    extract
    zsh-autosuggestions
    zsh-syntax-highlighting
  )  
```
6. Adicionar alias e functions customs (ver Referencia)
7. Realizar customização avançada se necessário por meio de  https://github.com/Powerlevel9k/powerlevel9k
8. Criar comandos utilitários

```
alias zcfg="source ~/.zshrc"

function kubecfg() {
  if [ -z $1 ]
  then
     kubectl config current-context
  else	
     cp $HOME/.kube/config_$1 $HOME/.kube/config 
     kubectl config current-context
  fi	
}


alias mkctl='microk8s.kubectl'
alias mhelm='microk8s.helm'
alias mstart='microk8s.start'
alias mstop='microk8s.stop'
alias mstatus='microk8s.status'
```

# Terminal

Configurar Terminator como default Terminal

## Fontes

Adicionar Powerline Fonts para serem utilizados pelo terminal

```
cd software
git clone https://github.com/powerline/fonts.git
./install.sh
```
A seguir,
1.  navegar em **terminator -> preferences -> profiles**
1.  desmarcar _[ ] use the system fixed_
2.  selecionar a fonte.  "Roboto Mono for PowerLine Regular"
2. setar tamanho para 11


'Droid Sans Mono', 'monospace', monospace, 'Droid Sans Fallback'

## Themes
instalar o tema do iTerm:

`git clone https://github.com/mbadolato/iTerm2-Color-Schemes`

Entrar na pasta "terminator" e selecionar o "Monokai Vivid.config" ou "Dark Solarized Patched.config"

```
cd ~/.config/terminator
vim terminator

```
e adcionar o contéudo dos temas como configs para profile default

## shortcuts
* _ctrl+shift+t_ - cria nova aba
* _ctrl pgup/pgdown_ - altab entre os tabs
* _ctrl+shift+e_ - split vertical
* _ctrl+shift+o_ - split hor
* _alt+setas_ - alternar entre split wndows
* _ctrl+shift+ip/down_ - resize windows
* _ctrl+shift+x_ - zoom into window
* _ctrl+d / ctrl+w_- close
* _ctrl+r_ - search no historico de comandos, digitar uma parte do comando e a cada ctrl+l vai mostrar outas entradas no histórico corresponte ao search
* _ctrl+s_ - fwd-i-search


# Referência

## Sobre Privacidade
https://ssd.eff.org/
https://www.privacytools.io/

## alias e functions uteis em .zshrc

```
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias gbr="git branch"
alias gps="git push"
alias gpl="git pull"
alias gcm="git commit -m"
alias gcam="git commit -a -m"
alias gst="git status"
alias gdf="git diff"
alias gad="git add"
alias gch="git checkout"
alias gchb="git checkout -b"

alias zcfg="source ~/.zshrc"
alias mkctl='microk8s.kubectl'

function gacp() {
  git add .
  git commit -m "$1"
  git push
}
```

# Security & Privacy

## Browser Extension

Ad-Blocker: https://ublockorigin.com/

## Password Manager

https://bitwarden.com/download/ 

Configurar a conta

## VeraCrypt

https://www.veracrypt.fr/en/Downloads.html

Fazer downloas do .deb correspondente a versão

Seguir tutorial: https://www.veracrypt.fr/en/Beginner%27s%20Tutorial.html

Usado 256 MB com Ext4 (para Linux somente)

## Proton VPN

https://protonvpn.com/support/linux-vpn-setup/#linux_app
https://protonvpn.com/support/official-linux-vpn-mint/

## Links com artigos utilizados

* https://www.tecmint.com/install-linux-mint-alongside-windows-dual-boot-uefi-mode/
* https://www.youtube.com/watch?v=iwH1XqVjZOE
* https://medium.com/@chaudharypulkit93/my-coding-environment-setup-from-scratch-on-ubuntu-18-04-cedf30981042
* geral -  https://deepu.tech/my-beautiful-linux-development-environment/
* Gnome Extensions - https://deepu.tech/must-have-gnome-plugins/
* https://www.sitepoint.com/zsh-tips-tricks/
* https://medium.com/@ferhatsukrurende/terminator-zsh-ohmyzsh-58ba4303bd09
* https://deepu.tech/make-the-most-out-of-vscode/
* https://github.com/Powerlevel9k/powerlevel9k/wiki/Show-Off-Your-Config
* https://medium.com/@gutoinfo.ribeiro/instalando-e-configurando-o-zsh-no-ubuntu-20-04-4ef8a2499ed5
* https://brunopaz.dev/blog/my-linux-development-envrionment-2018/  







    





