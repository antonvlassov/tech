# Ethereum 

## Install Components

```
sudo add-apt-repository ppa:ethereum/ethereum 
sudo apt update 
sudo apt install solc
sudo apt install ethereum
```

## Solidity

* gerar binário: `solc --optimize --bin SmartContract.sol`
* gerar abi: `solc --abi SmartContract.sol`

## Go Ethereum

https://geth.ethereum.org/downloads/

# Serviços de Apoio ao Desenvolvimento (e Produção)

Criar uma conta de testes em diversos serviços que ajudam no desenvolvimento:

* **Etherscan** - api para serviços de verificação do smart contract
* **CoinMarketCap** - utilização de API para serviços de cotação
* **Pinata e Nft.Storage** - serviços de IPFS (remote pinning)
* **Alchemy, QuickNode, Moralis, Infura** - hosted nodes em testnest, mainnet, etc 


# HardHat Dev Env

## Install Dependencias de Dev

Instalar yarn

```
npm install -g yarn
yarn init

```

Realizar a configuração para cada projeto de smart contract:
`yarn add --dev @nomiclabs/hardhat-ethers@npm:hardhat-deploy-ethers ethers @nomiclabs/hardhat-etherscan @nomiclabs/hardhat-waffle chai ethereum-waffle hardhat hardhat-contract-sizer hardhat-deploy hardhat-gas-reporter prettier prettier-plugin-solidity solhint solidity-coverage dotenv @openzeppelin/contracts path base64-sol nft.storage mime`

## Solc Select
Gerenciador de versões do compilador do Solidity, da mesma forma do **n** para Node.js ou **sdk-man** para Java

```
sudo apt install python3-pip
pip3 install solc-select

```

`solc-select` é instalado em `/home/anton/.local/bin` (essa pasta deve ser adicionada ao `$PATH`)

Comandos:

```
solc-select install 0.8.8
solc-select use 0.8.8

```

## Slither (Smart Contract Audit)

```
sudo apt install python3-pip
pip3 install slither-analyzer 

```

Como gerar relatório dos smart contracts na pasta `./contracts`

`slither ./contracts --solc-remaps '@openzeppelin=node_modules/@openzeppelin' --exclude naming-convention,external-function,low-level-calls`




# Truffle Suite

## Ganache

Acessar https://www.trufflesuite.com/ganache

- AppImage é um arquivo que contém o software e todas as suas dependências pronto para a execução
- AppImage - não há instalação, extração, etc é um software completo
- executa com permissões do usuário comum, não altera nenhum arquivo ou conf do sistema que exige "root"
- read-only - naõ periste estado, assim que app é desligado, todo que foi feito é perdido


1. fazer download do ganache-2.3.0-linux-x86_64.AppImage
2. transformar em "executável"
3. clicar e rodar

## Truffle 

### Truffle Commands


### React Box

Install Truffle e os utilitários para Dev com React Box
```
sudo apt install build-essential
npm install -g truffle
npm install --save @truffle/hdwallet-provider
npm install --save-dev @openzeppelin/contracts
npm install --save chai chai-bn chai-as-promised
npm install --save dotenv  
npm install --save react-router-dom 

```

### Material UI
Para utilização de componentes React seguindo o padrão do Material UI

```
npm install --save @material-ui/core
npm install --save @material-ui/icons
npm install --save @material-ui/styles

```
Adcionar dentro do client/public/index.html:

```
<link rel="stylesheet"
  href=“https://fonts.googleapis.com/css?family=Roboto:400,500">
<link rel="stylesheet"
  href=“https://fonts.googleapis.com/icon?family=Material+Icons">

```
* https://material-ui.com/getting-started/installation/
* https://reactgo.com/material-ui-react-tutorial/
* https://material-ui.com/guides/composition/#link (integração com React Router)


### Utilização

Use:
- `truffle init` - cria uma estrutra básica para desenvolvimento de smart contract, organizado por pastas para "smart contract", testes e scripts utlitários interessantes para automação de processo de dev do smart contract, 
- `truffle unbox react` <- cria um template para dev de Solidity e de Dapp FE com React (chamamdo "Truffle React Box") - https://www.trufflesuite.com/boxes/react, catalogo de boxes: https://www.trufflesuite.com/boxes. Já realiza download de todas as dependências (web3js)

## Truffle HD PRovider

install

`npm install --save @truffle/hdwallet-provider`

Configure `truffle-config.js`:
  
```
  provider_ganache: {
      provider: function () {
        return new HDWalletProvider(Mnemonic, "http://127.0.0.1:7545", AccountIndex);
      },
      network_id: "5777",
    }

```


# Node

## Web3js
```
npm install web3 --save
npm install web3.js-browser --save

```
Teste de integração do Web3 com Ganache (ou qquer outra rede acessível):

Utilizar quaisquer 2 accounts criados pelo Ganache (exemplo):
* addr1: 0xce06980eE46B10389799651587DBe22c72Dc2f4a
* addr2: 0xFAD9BfD6cac619B1734516bf8C220c922d9A43D8

`node`

**>**`let Web3 = require('web3'); //attention CAPITAL Web3`

**>**`let web3 = new Web3(new Web3.providers.HttpProvider('HTTP://127.0.0.1:7545'));`

**>**`web3.eth.getBalance("0xce06980eE46B10389799651587DBe22c72Dc2f4a").then(result => console.log(web3.utils.fromWei(result, "ether")));`

**>**`web3.eth.sendTransaction({from: "0xce06980eE46B10389799651587DBe22c72Dc2f4a", to: "0xFAD9BfD6cac619B1734516bf8C220c922d9A43D8", value: web3.utils.toWei("1","ether")});`

