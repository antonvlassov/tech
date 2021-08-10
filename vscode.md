# Shortcuts

* `ctrl+,` - abre settings
* `ctrl+b` - fecha side bar
* `ctrl+shift+e` - explorer
* `ctrl+shift+d` - debug
* `ctrl+shift+f` - search bar, pode fazer replace em varios arquivos, mostra preview
* `ctrl+shift+p` - command pallete
* `ctrl+p` - busca arquivo dentro do projeto aberto, para abrir arquivos (pode manter o explorer fechado)
* `ctrl+f / cms+h` -  find/replace
* `ctrl+w` - fecha arquivo
* `ctrl+tab` - navegar entre as abas abertas
* `ctrl+c` - nao precisa seleiconar a linha, basta posicionar o cursor que linha inteira será selecionada
* `ctrl+d` - seleciona a palavra inteira do cursor (depois ctrl+f para busca)
* `ctrl+home/end` - começo ou fim do arquivo


# Setting

## Setting Sync

`Turn On Setting Sync` - nova feature que permite configurar conta do github para fazer upload e sync de settings

## Configurações Comuns

Algumas outras configurações comuns

* `files.defaultLanguage` - markdown ou ${activeEditorLanguage}
* `font Family` - selecionar o tipo
* `font Ligatures` - transforma sinais (==, =>) em imagens graficos
* `font Size` - tamanho da fonte etc
* `zoom level` - habilitar ou não controle de zoom com ctrl + scroll do mouse
* `word wrap on` - word wrap column (permite definir quando fazer wrap_)
* `files > exclude` - quais arquivos não mostrar dentro do editor (hidden)
* `cursor` - definir formato de blinking, tipo, etc

Fonte Powerline9k para deixar o terminal do VSCode semelhante ao do Terminator/OhMyZsh/Agnostic Theme:

1. Acessar "Setting", Terminal > Integrated:Font Family
2. Adicionar: `Roboto Mono for Powerline`

# Terminal
Font Family, Font Size, etc, cursor, etc

# Snippets

* [JavaScript Code Snippets](https://github.com/xabikos/vscode-javascript/blob/master/snippets/snippets.json): `code --install-extension xabikos.javascriptsnippets`

* [React ES7 Code Snippets](https://github.com/dsznajder/vscode-es7-javascript-react-snippets): `code --install-extension dsznajder.es7-react-js-snippets`

# Themes
`ctrl+shift+p` > `"themes"` > `Color Theme (enter)` -  selecionar temas, que podem ser as defaults ou instaladas como extensões. Ao selecionar o team, poderá verificar como ficará o VSCode. 

Algumas temas comuns:

* dark + (default dark)
* night owl
* winter is coming
* zeppelin


# UI Org

* **pin tab** - permite marcar um tab para não fechar após "close all" 

* **grid system** - organizar arquvios e linhas e colunas arrastando do explorer para area de trabalho

* **Zen View Mode** - elimina todos os paineis, e deixa somente o codigo-fonte aberto
* **Toggle Minimap** - habilita ou desabilita visualizador de um arquivo muito grande

## Extensions

Extensões recomendadas:
```
code --install-extension redhat.java
code --install-extension secanis.jenkinsfile-support
code --install-extension redhat.vscode-yaml
code --install-extension ms-kubernetes-tools.vscode-kubernetes-tools
code --install-extension codezombiech.gitignore
code --install-extension mdickin.markdown-shortcuts
code --install-extension juanblanco.solidity
code --install-extension tintinweb.solidity-visual-auditor
code --install-extension ibmblockchain.ibm-blockchain-platform
code --install-extension oshri6688.javascript-test-runner
code --install-extension secanis.jenkinsfile-support
code --install-extension mikestead.dotenv
code --install-extension pnp.polacode
code --install-extension esbenp.prettier-vscode  
code --install-extension tintinweb.vscode-inline-bookmarks
code --install-extension oderwat.indent-rainbow
code --install-extension pkief.material-icon-theme
code --install-extension aaron-bond.better-comments
code --install-extension johnpapa.vscode-cloak
code --install-extension patbenatar.advanced-new-file
code --install-extension formulahendry.auto-rename-tag
code --install-extension humao.rest-client
code --install-extension pranaygp.vscode-css-peek
code --install-extension ecmel.vscode-html-css
code --install-extension ritwickdey.liveserver
code --install-extension wallabyjs.quokka-vscode
code --install-extension editorconfig.editorconfig
code --install-extension dsznajder.es7-react-js-snippets
code --install-extension wix.vscode-import-cost
code --install-extension yzane.markdown-pdf
code --install-extension eamodio.gitlens
code --install-extension ms-vsliveshare.vsliveshare
```


# Build In
**emmet** - `!`  - emmet abbreviations para HTML e CSS

começar alguns códigos para gerar HTML e CSS rapidamente por meio de shortcode /snippet
