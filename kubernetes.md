# Minikube
minikube start --vm-driver=virtualbox
minikube ip
minikube start --cpus 4 --memory 4096 --vm-driver=virtualbox

minikube start --cpus 4 --memory 4096 --vm-driver=virtualbox --extra-config=apiserver.authorization-mode=RBAC --extra-config=apiserver.admission-control="NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,ResourceQuota,PodPreset,PodSecurityPolicy"

minikube start -p <name> <-- nome do novo cluster - permite criara varios clusters minikube para propositos de teste

minikube ssh
loga no worker node (no caso do minukibe eh unico node)
docker ps dentro do node lista os processos

minikube dashboard

minikube addons enable metrics-server <-- precisa instalar o addon metrics-server para testes de auto-scaling no minikube

# Basicos
https://kubernetes.io/docs/reference/kubectl/overview/

Aplicação, consulta

```
kubectl apply -f client-node-port.yaml
kubectl apply -f <pasta>
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/mandatory.yaml
kubectl delete -f client-pod.yaml
kubectl delete deployment client-deployment
kubectl delete service client-node-port
kubectl delete -f <pasta>

kubectl get pods <-- status dos objetos do tipo Pods

kubectl get pods --watch <-- monitora o status dos pods e mostra a criação etc

kubectl get services <--- tipo Service

kubectl describe pod client-pod
kubectl describe <kind do objeto> <nome do objeto>

kubectl get pods -o wide

kubectl get pv
kubectl get pvc
kubectl get storageclass

kubectl get pods -n kube-system | grep nginx-ingress-controller
kubectl get pods --all-namespaces | grep nginx

kubectl get endpoints

kubectl get ds  <- lista dos DaemonSets

```

Executar e expor imperativo

```
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.10 --port=8080
kubectl expose deployment hello-minikube --type=NodePort

kubectl expose deployment simple-nlp-app-deployment --type=NodePort
minikube service simple-nlp-app-deployment --url

kubectl port-forward fortune 8080:80 <-- permite expor um Pod via portforwarding pelo kubectl (localhost)

kubectl port-forward svc/my-release-postgresql 5432:5432  <-- port forward para um Service em vez do Pod    

kubectl expose deployment kubia-deployment --type=LoadBalancer --name=kubia-lb <-- cria um Service do tipo LoadBalance para Deployment




```

Acesso via Proxy

```
-- como acessar pods rapidamente pelo proxy
kubectl proxy <-- faz kubectl subir um server local para fazer pont com api server
cada recurso possui uma API gerada dentro do API server que serve como proxy para seu acesso
http://localhost:8001/api/v1/namespaces/default/pods/stateful-app-stateful-set-0/proxy/

curl -X POST -d "Hello pod" localhost:8001/api/v1/namespaces/default/pods/stateful-app-stateful-set-0/proxy/

curl localhost:8001/api/v1/namespaces/default/pods/stateful-app-stateful-set-1/proxy/

curl -X POST -d "Hello pod 1" localhost:8001/api/v1/namespaces/default/pods/stateful-app-stateful-set-1/proxy/
```

Acessar Logs e Pods
```
- acessar logs gerados pelo container em pod
kubectl logs client-deployment-6567d98b68-jntjn
kubectl exec -it client-deployment-6567d98b68-jntjn sh

kubectl exec -it -n kube-system nginx-ingress-controller-7c66d668b-crdfh cat /etc/nginx/nginx.conf

kubectl logs nginx-ingress-controller-7c66d668b-crdfh -n ingress-nginx

kubectl exec fortune-configmap -it -c web-server sh <-- acessa um container especifico do multi-container Pod e executa "sh" la dentro

kubectl logs fortune-configmap -c web-server <-- visualiza os logs do container especifico no pod multi-container 

kubectl logs mypod --previous

kubectl exec kubia-2r1qb -- touch /var/ready   <-- cria um arquivo vazio no Pod

```

Secrets, Configmaps Segurança
```
kubectl create secret generic pgpassword --from-literal PGPASSWORD=1234asdf

kubectl create configmap my-config --from-file=configs/env-file.conf

kubectl create configmap fortune-config-volume --from-file=configs/multi-file <-- de uma pasta com multioplos arquivos cada um de um tipo

kubectl edit configmap fortune-config-volume  <-- permite editar via ediyor de texto default associado ao shell, caso contrario deletear e recriar o config map

```

Consultar API / manual

```
kubectl explain pods <- doc resumido sobre os campos e referência para o o link do manual
```

# Avanaçados

Security

```
kubectl get sa
kubectl describe sa sa-test
kubectl create serviceaccount sa-test
kubectl describe secret sa-test-token-5tmmd
```

Pods com shell interativo para testes

```
-- testes rapidos de Pods para Pod, Services, ping, etc
kubectl run busybox --image=busybox -it --rm --restart=Never  <-- executa o pod busybox, e o remove quando sai do shell

kubectl run busybox --image=busybox -it --restart=Never -- sh <-- abre shell

kubectl attach busybox-c8f8564d4-q7mmb -c busybox -i -t <-- volta a sessai interativa do console para o busybox

kubectl exec kubia-7nog1 -- curl -s http://10.111.249.153

parte "--" do comando: indica que os parametros do 'kubectl' terminarem, e o que vem depois são comandos para executar dentro do container

- com especeificação de recursos
kubectl run requests-pod-2 --image=busybox --restart Never --requests='cpu=800m,memory=20Mi' --command=dd if=/dev/zero of=/dev/null
kubectl run requests-pod-2 --image=busybox --restart Never --requests='cpu=800m,memory=20Mi' -- dd if=/dev/zero of=/dev/null

-- executar o load generator
kubectl run -it --rm --restart=Never loadgenerator --image=busybox -- sh -c "while true; do wget -O - -q http://kubia-hpa-cluster-ip; done"

```

Labels

```
kubectl label node minikube gpu=true <-- adiciona label aos nodes para especificar clusters nao homogenios, 

nodeSelector:   <-- sintaxe no yaml que especifca os nodes
  gpu: "true"

kubectl label node minikube disk=hhd --overwrite <-- troca o label

kubectl describe nodes

kubectl get pods --show-labels <-- mostra os labels customs definidos no metadata (app=app_name, rel=release_type)

kubectl get po -l creation_method=manual  <- listar pods com base me seletores creation_method!=manual env in (prod,devel) env notin (prod,devel)


```

Manutenção de Worker Node

```
kubectl cordon <node>  <-- impede que novos pods seja alocados no node
kubectl drain <node>  <-- realoca os pods que estão execução no node "blqoeuado" para outros nodes disponiveis
kubectl uncordon <node>  <-- libers novamente o node para receber pods  
```

HPA

```
kubectl describe hpa
kubectl get hpa --watch  <- - mostra os evetos gerados pelo auto-scaler
kubectl get deployment -o wide --watch <- - mostra o impacto do autoscaler no Deployment

```

# Resumo dos Comandos

```
# use multiple kubeconfig files at the same time and view merged config
KUBECONFIG=~/.kube/config:~/.kube/kubconfig2 kubectl config view

# get the password for the e2e user
kubectl config view -o jsonpath='{.users[?(@.name == "e2e")].user.password}'

kubectl config view -o jsonpath='{.users[].name}'    # get a list of users
kubectl config get-contexts                          # display list of contexts 
kubectl config current-context			               # display the current-context
kubectl config use-context my-cluster-name           # set the default context to my-cluster-name

# add a new cluster to your kubeconf that supports basic auth
kubectl config set-credentials kubeuser/foo.kubernetes.com --username=kubeuser --password=kubepassword

# permanently save the namespace for all subsequent kubectl commands in that context.
kubectl config set-context --current --namespace=ggckad-s2

# set a context utilizing a specific username and namespace.
kubectl config set-context gce --user=cluster-admin --namespace=foo \
  && kubectl config use-context gce
 
kubectl config unset users.foo                       # delete user foo

kubectl apply -f ./my-manifest.yaml           # create resource(s)
kubectl apply -f ./my1.yaml -f ./my2.yaml     # create from multiple files
kubectl apply -f ./dir                        # create resource(s) in all manifest files in dir
kubectl apply -f https://git.io/vPieo         # create resource(s) from url
kubectl create deployment nginx --image=nginx  # start a single instance of nginx
kubectl explain pods,svc                       # get the documentation for pod and svc manifests

# Get commands with basic output
kubectl get services                          # List all services in the namespace
kubectl get pods --all-namespaces             # List all pods in all namespaces
kubectl get pods -o wide                      # List all pods in the namespace, with more details
kubectl get deployment my-dep                 # List a particular deployment
kubectl get pods --include-uninitialized      # List all pods in the namespace, including uninitialized ones
kubectl get pod my-pod -o yaml                # Get a pod's YAML
kubectl get pod my-pod -o yaml --export       # Get a pod's YAML without cluster specific information

# Describe commands with verbose output
kubectl describe nodes my-node
kubectl describe pods my-pod

kubectl get services --sort-by=.metadata.name # List Services Sorted by Name

# List pods Sorted by Restart Count
kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'

# Get the version label of all pods with label app=cassandra
kubectl get pods --selector=app=cassandra rc -o \
  jsonpath='{.items[*].metadata.labels.version}'

# Get all worker nodes (use a selector to exclude results that have a label
# named 'node-role.kubernetes.io/master')
kubectl get node --selector='!node-role.kubernetes.io/master'

# Get all running pods in the namespace
kubectl get pods --field-selector=status.phase=Running

# Get ExternalIPs of all nodes
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'

# List Names of Pods that belong to Particular RC
# "jq" command useful for transformations that are too complex for jsonpath, it can be found at https://stedolan.github.io/jq/
sel=${$(kubectl get rc my-rc --output=json | jq -j '.spec.selector | to_entries | .[] | "\(.key)=\(.value),"')%?}
echo $(kubectl get pods --selector=$sel --output=jsonpath={.items..metadata.name})

# Show labels for all pods (or any other Kubernetes object that supports labelling)
# Also uses "jq"
for item in $( kubectl get pod --output=name); do printf "Labels for %s\n" "$item" | grep --color -E '[^/]+$' && kubectl get "$item" --output=json | jq -r -S '.metadata.labels | to_entries | .[] | " \(.key)=\(.value)"' 2>/dev/null; printf "\n"; done

# Check which nodes are ready
JSONPATH='{range .items[*]}{@.metadata.name}:{range @.status.conditions[*]}{@.type}={@.status};{end}{end}' \
 && kubectl get nodes -o jsonpath="$JSONPATH" | grep "Ready=True"

# List all Secrets currently in use by a pod
kubectl get pods -o json | jq '.items[].spec.containers[].env[]?.valueFrom.secretKeyRef.name' | grep -v null | sort | uniq

# List 
 sorted by timestamp
kubectl get events --sort-by=.metadata.creationTimestamp

kubectl set image deployment/frontend www=image:v2               # Rolling update "www" containers of "frontend" deployment, updating the image
kubectl rollout undo deployment/frontend                         # Rollback to the previous deployment
kubectl rollout status -w deployment/frontend                    # Watch rolling update status of "frontend" deployment until completion

# deprecated starting version 1.11
kubectl rolling-update frontend-v1 -f frontend-v2.json           # (deprecated) Rolling update pods of frontend-v1
kubectl rolling-update frontend-v1 frontend-v2 --image=image:v2  # (deprecated) Change the name of the resource and update the image
kubectl rolling-update frontend --image=image:v2                 # (deprecated) Update the pods image of frontend
kubectl rolling-update frontend-v1 frontend-v2 --rollback        # (deprecated) Abort existing rollout in progress

cat pod.json | kubectl replace -f -                              # Replace a pod based on the JSON passed into std

# Force replace, delete and then re-create the resource. Will cause a service outage.
kubectl replace --force -f ./pod.json

# Create a service for a replicated nginx, which serves on port 80 and connects to the containers on port 8000
kubectl expose rc nginx --port=80 --target-port=8000

# Update a single-container pod's image version (tag) to v4
kubectl get pod mypod -o yaml | sed 's/\(image: myimage\):.*$/\1:v4/' | kubectl replace -f -

kubectl label pods my-pod new-label=awesome                      # Add a Label
kubectl annotate pods my-pod icon-url=http://goo.gl/XXBTWq       # Add an annotation
kubectl autoscale deployment foo --min=2 --max=10                # Auto scale a deployment "foo"

kubectl scale --replicas=3 rs/foo                                 # Scale a replicaset named 'foo' to 3
kubectl scale --replicas=3 -f foo.yaml                            # Scale a resource specified in "foo.yaml" to 3
kubectl scale --current-replicas=2 --replicas=3 deployment/mysql  # If the deployment named mysql's current size is 2, scale mysql to 3
kubectl scale --replicas=5 rc/foo rc/bar rc/baz                   # Scale multiple replication controllers

kubectl delete -f ./pod.json                                              # Delete a pod using the type and name specified in pod.json
kubectl delete pod,service baz foo                                        # Delete pods and services with same names "baz" and "foo"
kubectl delete pods,services -l name=myLabel                              # Delete pods and services with label name=myLabel
kubectl delete pods,services -l name=myLabel --include-uninitialized      # Delete pods and services, including uninitialized ones, with label name=myLabel
kubectl get pods -Lapp -Ltier -Lrole     # cria tabela com colunas com valores dos labels
kubectl -n my-ns delete po,svc --all                                      # Delete all pods and services, including uninitialized ones, in namespace my-ns,
# Delete all pods matching the awk pattern1 or pattern2
kubectl get pods  -n mynamespace --no-headers=true | awk '/pattern1|pattern2/{print $1}' | xargs  kubectl delete -n mynamespace pod

kubectl logs my-pod                                 # dump pod logs (stdout)
kubectl logs -l name=myLabel                        # dump pod logs, with label name=myLabel (stdout)
kubectl logs my-pod --previous                      # dump pod logs (stdout) for a previous instantiation of a container
kubectl logs my-pod -c my-container                 # dump pod container logs (stdout, multi-container case)
kubectl logs -l name=myLabel -c my-container        # dump pod logs, with label name=myLabel (stdout)
kubectl logs my-pod -c my-container --previous      # dump pod container logs (stdout, multi-container case) for a previous instantiation of a container
kubectl logs -f my-pod                              # stream pod logs (stdout)
kubectl logs -f my-pod -c my-container              # stream pod container logs (stdout, multi-container case)
kubectl logs -f -l name=myLabel --all-containers    # stream all pods logs with label name=myLabel (stdout)
kubectl run -i --tty busybox --image=busybox -- sh  # Run pod as interactive shell
kubectl attach my-pod -i                            # Attach to Running Container
kubectl port-forward my-pod 5000:6000               # Listen on port 5000 on the local machine and forward to port 6000 on my-pod
kubectl exec my-pod -- ls /                         # Run command in existing pod (1 container case)
kubectl exec my-pod -c my-container -- ls /         # Run command in existing pod (multi-container case)
kubectl top pod POD_NAME --containers               # Show metrics for a given pod and its containers

kubectl cordon my-node                                                # Mark my-node as unschedulable
kubectl drain my-node                                                 # Drain my-node in preparation for maintenance
kubectl uncordon my-node                                              # Mark my-node as schedulable
kubectl top node my-node                                              # Show metrics for a given node
kubectl cluster-info                                                  # Display addresses of the master and services
kubectl cluster-info dump                                             # Dump current cluster state to stdout
kubectl cluster-info dump --output-directory=/path/to/cluster-state   # Dump current cluster state to /path/to/cluster-state

# If a taint with that key and effect already exists, its value is replaced as specified.
kubectl taint nodes foo dedicated=special-user:NoSchedule

kubectl api-resources --namespaced=true      # All namespaced resources
kubectl api-resources --namespaced=false     # All non-namespaced resources
kubectl api-resources -o name                # All resources with simple output (just the resource name)
kubectl api-resources -o wide                # All resources with expanded (aka "wide") output
kubectl api-resources --verbs=list,get       # All resources that support the "list" and "get" request verbs
kubectl api-resources --api-group=extensions # All resources in the "extensions" API group

```


