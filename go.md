# Install

```
sudo apt install golang-go
go version

```

# Config

```
mkdir -p ~/go/src/test

```
criar GOPATH no .zshrc apontando para raiz da pasta `/go`
go compila as fontes se estiverem na estrutura pre-definida (ver `go help gopath`) embaixo da pasta raiz apontada pelo GOPATH
pela convenção, é usado `$HOME/go`



# Test

`cd go/src/test `

`vim test.go`

Criar seguinte conteúdo;

```
package main
import "fmt"
func main() {
    fmt.Printf("Hello, Gophers\n")
}

```

Depois buildar e executar:

```
go build test.go 
./test  

```