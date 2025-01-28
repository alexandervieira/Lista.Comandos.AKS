# Azure Kubernetes Services

## Instalação

    kubectl (https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/);

## Instalar Kubectl
    az aks install-cli

## Ajuda

#### kubectl

    kubectl --help

	Basic Commands (Beginner):
    create          Create a resource from a file or from stdin
    expose          Take a replication controller, service, deployment or pod and expose it as a new Kubernetes service
    run             Run a particular image on the cluster
    set             Set specific features on objects

    Basic Commands (Intermediate):
      explain         Get documentation for a resource
      get             Display one or many resources
      edit            Edit a resource on the server
      delete          Delete resources by file names, stdin, resources and names, or by resources and label selector
    
    Deploy Commands:
      rollout         Manage the rollout of a resource
      scale           Set a new size for a deployment, replica set, or replication controller
      autoscale       Auto-scale a deployment, replica set, stateful set, or replication controller
    
    Cluster Management Commands:
      certificate     Modify certificate resources
      cluster-info    Display cluster information
      top             Display resource (CPU/memory) usage
      cordon          Mark node as unschedulable
      uncordon        Mark node as schedulable
      drain           Drain node in preparation for maintenance
      taint           Update the taints on one or more nodes
    
    Troubleshooting and Debugging Commands:
      describe        Show details of a specific resource or group of resources
      logs            Print the logs for a container in a pod
      attach          Attach to a running container
      exec            Execute a command in a container
      port-forward    Forward one or more local ports to a pod
      proxy           Run a proxy to the Kubernetes API server
      cp              Copy files and directories to and from containers
      auth            Inspect authorization
      debug           Create debugging sessions for troubleshooting workloads and nodes
      events          List events
    
    Advanced Commands:
      diff            Diff the live version against a would-be applied version
      apply           Apply a configuration to a resource by file name or stdin
      patch           Update fields of a resource
      replace         Replace a resource by file name or stdin
      wait            Experimental: Wait for a specific condition on one or many resources
      kustomize       Build a kustomization target from a directory or URL
    
    Settings Commands:
      label           Update the labels on a resource
      annotate        Update the annotations on a resource
      completion      Output shell completion code for the specified shell (bash, zsh, fish, or powershell)
    
    Subcommands provided by plugins:
    
    Other Commands:
      api-resources   Print the supported API resources on the server
      api-versions    Print the supported API versions on the server, in the form of "group/version"
      config          Modify kubeconfig files
      plugin          Provides utilities for interacting with plugins
      version         Print the client and server version information

##### Comando específico
	kubectl apply --help
	kubectl describe --help
	kubectl <qualquer_comando_kubectl> --help

## Configuração   

### Geral

##### Print the client and server version information
    kubectl version	

##### List the requested object(s) across all namespaces.
	kubectl get pods -A
	
##### Modify kubeconfig files
	kubectl config get-contexts	
	kubectl config set-context  --current --namespace=kube-system
	kubectl config get-clusters

##### Create Image
	docker image build -t bookstore-app:1.0.0 -f .\src\BookStore.WebApi\Dockerfile .

##### Create namespaces
    kubectl create namespace backend

##### List namespaces
	kubectl get all

##### List namespaces
	kubectl get ns

##### Get nodes
    kubectl get nodes

##### Get pods
    kubectl get pods

##### Get deployments
    kubectl get deployments

##### Get services
    kubectl get services

##### Get secrets
    kubectl get secrets	

##### Apply deployments
    kubectl apply -f .\deployment.yaml -n backend

##### Apply services
    kubectl apply -f .\service.yaml -n backend

##### Delete deployments
    kubectl delete -f .\deployment.yaml -n backend

##### Delete services
    kubectl delete -f .\service.yaml -n backend

##### Set replicas
    kubectl scale deploy bookstore --replicas=2 -n backend

##### List logs
    kubectl logs etcd-minikube

##### AKS Básico

    az login
    
    rg=rg-aks-cli
    local=eastus
    aks=aks-cli
    
    az group create -n $rg -l $local
    
    az provider register --namespace Microsoft.ContainerService
    
    az provider show --namespace Microsoft.ContainerService --query     "registrationState"
    
    az aks create -g $rg -n $aks --generate-ssh-keys
    
    az aks list -o yaml
    
    az aks browser -g $rg -n $aks
    
    az aks show -g $rg -n $aks
    
    az aks stop -g $rg -n $aks
    
    az aks start -g $rg -n $aks
    
    az aks delete -g $rg -n $aks
    
    az group delete -g $rg

##### AKS Detalhado

    az login
    
    rg=rg-aks-cli
    local=eastus
    aks=aks-cli-detalhado
    sku=Standard_B2s
    
    az aks get-versions --location $local --output table
    
    az group create -n $rg -l $local
    
    az aks create -g $rg -n $aks --enable-aad --disable-local-accounts     --node-count 3 --node-vm-size $sku
    
    az aks create -g $rg -n $aks --enable-aad --node-count 3 --node-vm-size $sku
    
    az aks list -o yaml
    
    az aks browser -g $rg -n $aks
    
    az aks show -g $rg -n $aks -o yaml
    
    az aks show -g $rg -n $aks --query {AzureRBAC:aadProfile} -o yaml
    
    az aks update -g $rg -n $aks --enable-azure-rbac
    
    az aks update -g $rg -n $aks --enable-local-accounts
    
    az aks stop -g $rg -n $aks
    
    az aks start -g $rg -n $aks
    
    az aks delete -g $rg -n $aks
    
    az group delete -g $rg

##### AKS vinculado ao ACR

    az login
    
    rg=rg-akscli
    local=eastus
    aks=aks-cli
    acr=acrakstestcli
    
    az group create -n $rg -l $local
    
    az aks create -g $rg -n $aks
    
    az acr create -n $acr -g $rg --sku basic
    
    az aks check-acr -n $aks -g $rg --acr $acr.azurecr.io
    
    az aks update -g $rg -n $aks --attach-acr $acr
    
    #az provider register --namespace Microsoft.ContainerService
    
    #az provider show --namespace Microsoft.ContainerService --query     "registrationState"
    
    #az aks create -g $rg -n $aks --generate-ssh-keys
    
    az aks list -o yaml
    
    az aks browser -g $rg -n $aks
    
    az aks show -g $rg -n $aks
    
    az aks stop -g $rg -n $aks
    
    az aks start -g $rg -n $aks
    
    az aks delete -g $rg -n $aks
    
    az group delete -g $rg

##### HTTP Application Routing

###### Uso recomendado para ambiente não produtivo.

    az login

    rg=rg-httpapp-routing
    local=eastus
    aks=aks-http-app-routing
    sku=Standard_B2s
    
    az group create -n $rg -l $local
    
    az aks create -g $rg -n $aks --node-count 1 --node-vm-size $sku --enable-addons http_application_routing
    
    az aks show -g $rg -n $aks --query addonProfiles.httpApplicationRouting -o yaml
    
    az aks show -g $rg -n $aks --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o yaml
    
    az aks get-credentials -n $aks -g $rg
    
    kubectl apply -f httpapp-routing-service.yaml
    
    kubectl apply -f httpapp-routing-deployment.yaml
    
    kubectl apply -f httpapp-rounting-ingress.yaml
    
    kubectl get pods -n kube-system
    
    kubectl logs -f deploy/addon-http-application-routing-external-dns -n kube-system
    
    kubectl logs -f deploy/addon-http-application-routing-nginx-ingress-controller -n kube-system
    
    az aks disable-addons -g $rg -n $aks --addons http_application_routing
    
    az group delete -n $rg --yes --no-wait

##### AKS e Kubectl - Applications: gestbook e azure-vote

    # Login no Azure 
    az login
    
    # Parametros
    rg=rg-basic
    local=eastus2
    aks=aks-basic
    sku=Standard_B2s    
    
    # Criar Grupo de Recursos
    az group create -n $rg -l $local
    
    # Criar AKS
    az aks create -g $rg -n $aks --node-count 1 --node-vm-size $sku 
    
    # Obter Credenciais
    az aks get-credentials -g $rg -n $aks
    
    # Obter Versdoes do Kubernetes
    kubectl version -o yaml
    
    # Informacoes do Cluster
    kubectl cluster-info
    
    # Configuracoes do Cluster
    kubectl config view
    
    # API Resources
    kubectl api-resources
    
    # API Version
    kubectl api-versions
    
    
    # GET - Todos os objetos de todos os namespaces
    kubectl get all
    kubectl get all --all-namespaces
    
    # GET - Obter Pods
    kubectl get pods
    kubectl get po
    kubectl get po --all-namespaces
    kubectl get pods --namespace kube-system
    
    # GET - Obter Namespaces
    kubectl get namespaces
    kubectl get ns
    kubectl get pods -n kube-system
    
    # GET - Obter Nodes
    kubectl get nodes
    kubectl get no
    kubectl get nodes -o wide
    
    # Criar POD com imagem NGINX
    kubectl run nginx --image=nginx
    kubectl get pods
    kubectl run busybox --image=busybox --restart=Never
    
    # Excluir Pod
    kubectl delete pod busybox
    
    # Listar Containers no POD
    kubectl get pods -o jsonpath="{.items[*].spec.containers[*].name}"
    
    # DESCRIBE - Descrever Objetos Kubernetes
    kubectl describe pod nginx
    kubectl get no
    kubectl describe node aks-nodepool1-30498416-vmss000000     #aks-nodepool1-16022034-vmss000000
    
    # LOGS - Obter Logs
    kubectl logs nginx
    kubectl logs nginx --since=30m nginx
    
    # Excluir Pods
    kubectl delete pod nginx
    kubectl get pods
    
    # guestbook + redis
    kubectl apply -f guestbook.yaml
    
    kubectl get pods -n guestbook
    
    kubectl get services -n guestbook
    
    kubectl get service guestbook -n guestbook
    
    kubectl delete -n guestbook -f guestbook.yaml
    
    kubectl create namespace azure-vote
    
    kubectl apply -f azure-deployments.yaml
    
    kubectl apply -f azure-services.yaml
    
    kubectl get services -n azure-vote
    
    kubectl get pods -n azure-vote
    
    kubectl delete -f azure-deployments.yaml
    
    kubectl delete -f azure-services.yaml
    
    kubectl delete -n azure-vote



## Contribuições

Sinta-se a vontade para realizar adicionar mais informações ou realizar correções. Fork me!
