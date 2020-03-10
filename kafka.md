# Instalação Local para Testes

TODO

# Instalação Básica para MicroK8S
Vai instalar um cluster com 3 brokers e zookeeper com 3 nodes no microk8s 

## Preparar Ambiente
Instalar microk8s, kubectl e helm (kubernetes.md)

## Preparar arquivo de configuração

Criar arquivo `helm-values-kafka.yaml` com seguinte conteúdo:

```
# Ref: https://hub.helm.sh/charts/incubator/kafka/0.20.8
imageTag: 5.3.1 
external:
   enabled: true
   # type can be either NodePort or LoadBalancer
   type: NodePort
   # annotations:
configurationOverrides:
   ##
   ## Setting "advertised.listeners" here appends to "PLAINTEXT://${POD_IP}:9092,", ensure you update the domain
   ## If external service type is Nodeport:
  # "advertised.listeners": |-
  #   EXTERNAL://kafka.cluster.local:$((31090 + ${KAFKA_BROKER_ID}))
   "advertised.listeners": |-
     EXTERNAL://kafka.cluster.local:$((31090 + ${KAFKA_BROKER_ID}))
   ## If external service type is LoadBalancer and distinct is true:
   # "advertised.listeners": |-
   #   EXTERNAL://kafka-$((${KAFKA_BROKER_ID})).cluster.local:19092
   ## If external service type is LoadBalancer and distinct is false:
   # "advertised.listeners": |-
   #   EXTERNAL://EXTERNAL://${LOAD_BALANCER_IP}:31090
   ## Uncomment to define the EXTERNAL Listener protocol
   # "listener.security.protocol.map": |-
   #   PLAINTEXT:PLAINTEXT,EXTERNAL:PLAINTEXT
   "listener.security.protocol.map": |-
     PLAINTEXT:PLAINTEXT,EXTERNAL:PLAINTEXT
```

## Preparar ambiente

alterar `/etc/hosts` para adicionar seguinte entrada:

`127.0.0.1 kafka.cluster.local`


## Instalar Kafka

```
helm repo add incubator http://storage.googleapis.com/kubernetes-charts-incubator
kubectl create ns kafka
helm install --name my-kafka --namespace kafka -f helm-values-kafka.yaml incubator/kafka 
kubectl get all -n kafka && kubectl get ingress -n kafka && kubectl get endpoints -n kafka
```
## Instalar utilitários

Instalar **KafkaCat:**

`sudo apt install kafkacat`

Referencia:
https://docs.confluent.io/current/app-development/kafkacat-usage.html

Instalar **Kafka Tool:**

```
cd ~/software
wget http://www.kafkatool.com/download2/kafkatool.sh
chmod +x kafkatool.sh
./kafkatoo.sh

```
instalar na pasta "software", depois configurar 
new connection -> advanced -> bootstrap servers

adicionar um dos broker que fazer parte do cluster e que foram expostos via NodePort durante a instalação do Kafka
Ex.: `kafka.cluster.local:31090`


# Instalação Completa da Confluent

TODO

