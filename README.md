# Create-Kubernetes-cluster
:computer: Create Kubernetes Cluster

### Cluster em funcionamento
Verifique se o Minikube está instalado corretamente, executando o comando minikube version:

```
minikube version
```

### Executar
Inicie o cluster executando o comando minikube start:

```
minikube start
```

<img src="minikube.png">

Excelente! Agora você tem um cluster Kubernetes em execução em seu terminal online. O Minikube iniciou uma máquina virtual para você e um cluster Kubernetes agora está sendo executado nessa VM.

### Versão do cluster
Para interagir com o Kubernetes durante este bootcamp, usaremos a interface de linha de comando, kubectl. Para verificar se o kubectl está instalado, você pode executar o comando kubectl version:

```
kubectl version
```

```
Client Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.4", GitCommit:"e87da0bd6e03ec3fea7933c4b5263d151aafd07c", GitTreeState:"clean", BuildDate:"2021-02-18T16:12:00Z", GoVersion:"go1.15.8", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.2", GitCommit:"faecb196815e248d3ecfb03c680a4507229c2a56", GitTreeState:"clean", BuildDate:"2021-01-13T13:20:00Z", GoVersion:"go1.15.5", Compiler:"gc", Platform:"linux/amd64"}
```

OK, o kubectl está configurado e podemos ver tanto a versão do cliente quanto a do servidor. A versão do cliente é a versão kubectl; a versão do servidor é a versão do Kubernetes instalada no mestre. Você também pode ver detalhes sobre a compilação.

### Detalhes do cluster
Vamos ver os detalhes do cluster. Faremos isso executando kubectl cluster-info:

```
kubectl cluster-info
```

Durante este tutorial, vamos nos concentrar na linha de comando para implantar e explorar nosso aplicativo. Para visualizar os nós no cluster, execute o comando kubectl get nodes:

```
kubectl get nodes
```

Este comando mostra todos os nós que podem ser usados para hospedar nossos aplicativos. Agora temos apenas um nó e podemos ver que seu status está pronto (está pronto para aceitar aplicativos para implantação).

### Usando kubectl para criar uma implantação

Objetivos
- Saiba mais sobre implantações de aplicativos.
- Implante seu primeiro aplicativo no Kubernetes com o kubectl.


### Implantações do Kubernetes
Assim que o seu cluster Kubernetes estiver em execução você pode implementar seu aplicativo em contêiners nele. Para fazer isso, você precisa criar uma configuração do tipo Deployment do Kubernetes. O Deployment define como criar e atualizar instâncias do seu aplicativo. Depois de criar um Deployment, o Master do Kubernetes agenda as instâncias do aplicativo incluídas nesse Deployment para ser executado em nós individuais do Cluster.

Depois que as instâncias do aplicativo são criadas, um Controlador do Kubernetes Deployment monitora continuamente essas instâncias. Se o nó que hospeda uma instância ficar inativo ou for excluído, o controlador de Deployment substituirá a instância por uma instância em outro nó no cluster. **Isso fornece um mecanismo de autocorreção para lidar com falhas ou manutenção da máquina**.

Em um mundo de pré-orquestração, os scripts de instalação costumavam ser usados ​​para iniciar aplicativos, mas não permitiam a recuperação de falha da máquina. Ao criar suas instâncias de aplicativo e mantê-las em execução entre nós, as implantações do Kubernetes fornecem uma abordagem fundamentalmente diferente para o gerenciamento de aplicativos.

### Implantar seu primeiro aplicativo no Kubernetes

<img src="module_02_first_app.svg">

Você pode criar e gerenciar uma implantação usando a interface de linha de comando do Kubernetes, Kubectl . O Kubectl usa a API Kubernetes para interagir com o cluster. Neste módulo, você aprenderá os comandos Kubectl mais comuns necessários para criar implantações que executam seus aplicativos em um cluster Kubernetes.

Quando você cria um Deployment, você precisa especificar a imagem do contêiner para seu aplicativo e o número de réplicas que deseja executar. Você pode alterar essas informações posteriormente, atualizando sua implantação; Módulos5 e 6 do bootcamp explica como você pode dimensionar e atualizar suas implantações.

Os aplicativos precisam ser empacotados em um dos formatos de contêiner suportados para serem implantados no Kubernetes

Para sua primeira implantação, você usará um aplicativo Node.js empacotado em um contêiner Docker.(Se você ainda não tentou criar um aplicativo Node.js e implantá-lo usando um contêiner, você pode fazer isso primeiro seguindo as instruções do tutorial Olá, Minikube!).

Agora que você sabe o que são implantações (Deployment), vamos para o tutorial online e implantar nosso primeiro aplicativo!

O objetivo deste cenário é ajudá-lo a implantar seu primeiro aplicativo no Kubernetes usando o kubectl. Você aprenderá o básico sobre kubectl cli e como interagir com seu aplicativo.
O terminal online é um ambiente Linux pré-configurado que pode ser usado como um console normal (você pode digitar comandos). Clicar nos blocos de código seguido do sinal ENTER executará esse comando no terminal.

### Noções básicas do kubectl
Digite kubectl no terminal para ver seu uso. O formato comum de um comando kubectl é: kubectl action resource. Isso executa a ação especificada (como criar, descrever) no recurso especificado (como nó, contêiner). Você pode usar --help após o comando para obter informações adicionais sobre possíveis parâmetros:

```
kubectl get nodes --help
```

```
$ kubectl get nodes --help
Display one or many resources

 Prints a table of the most important information about the specified resources. You can filter the list using a label selector and the --selector flag. If the desired resource type is namespaced you will only see results in your current namespace unless you pass --all-namespaces.

 Uninitialized objects are not shown unless --include-uninitialized is passed.

 By specifying the output as 'template' and providing a Go template as the value of the --template flag, you can filter the attributes of the fetched resources.

Use "kubectl api-resources" for a complete list of supported resources.

Examples:
  # List all pods in ps output format.
  kubectl get pods
  
  # List all pods in ps output format with more information (such as node name).
  kubectl get pods -o wide
  
  # List a single replication controller with specified NAME in ps output format.
  kubectl get replicationcontroller web
  
  # List deployments in JSON output format, in the "v1" version of the "apps" API group:
  kubectl get deployments.v1.apps -o json
  
  # List a single pod in JSON output format.
  kubectl get -o json pod web-pod-13je7
  
  # List a pod identified by type and name specified in "pod.yaml" in JSON output format.
  kubectl get -f pod.yaml -o json
  
  # List resources from a directory with kustomization.yaml - e.g. dir/kustomization.yaml.
  kubectl get -k dir/
  
  # Return only the phase value of the specified pod.
  kubectl get -o template pod/web-pod-13je7 --template={{.status.phase}}
  
  # List resource information in custom columns.
  kubectl get pod test-pod -o custom-columns=CONTAINER:.spec.containers[0].name,IMAGE:.spec.containers[0].image
  
  # List all replication controllers and services together in ps output format.
  kubectl get rc,services
  
  # List one or more resources by their type and names.
  kubectl get rc/web service/frontend pods/web-pod-13je7

Options:
  -A, --all-namespaces=false: If present, list the requested object(s) across all namespaces. Namespace in current context is ignored even if specified with --namespace.
      --allow-missing-template-keys=true: If true, ignore any errors in templates when a field or map key is missing in the template. Only applies to golang and jsonpath output formats.
      --chunk-size=500: Return large lists in chunks rather than all at once. Pass 0 to disable. This flag is beta and may change in the future.
      --field-selector='': Selector (field query) to filter on, supports '=', '==', and '!='.(e.g. --field-selector key1=value1,key2=value2). The server only supports a limited number of field queries per type.
  -f, --filename=[]: Filename, directory, or URL to files identifying the resource to get from a server.
      --ignore-not-found=false: If the requested object does not exist the command will return exit code 0.
  -k, --kustomize='': Process the kustomization directory. This flag can't be used together with -f or -R.
  -L, --label-columns=[]: Accepts a comma separated list of labels that are going to be presented as columns. Names are case-sensitive. You can also use multiple flag options like -L label1 -L label2...
      --no-headers=false: When using the default or custom-column output format, don't print headers (default print headers).
  -o, --output='': Output format. One of: json|yaml|wide|name|custom-columns=...|custom-columns-file=...|go-template=...|go-template-file=...|jsonpath=...|jsonpath-file=... See custom columns [http://kubernetes.io/docs/user-guide/kubectl-overview/#custom-columns], golang template [http://golang.org/pkg/text/template/#pkg-overview] and jsonpath template [http://kubernetes.io/docs/user-guide/jsonpath].
      --output-watch-events=false: Output watch event objects when --watch or --watch-only is used. Existing objects are output as initial ADDED events.
      --raw='': Raw URI to request from the server.  Uses the transport specified by the kubeconfig file.
  -R, --recursive=false: Process the directory used in -f, --filename recursively. Useful when you want to manage related manifests organized within the same directory.
  -l, --selector='': Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
      --server-print=true: If true, have the server return the appropriate table output. Supports extension APIs and CRDs.
      --show-kind=false: If present, list the resource type for the requested object(s).
      --show-labels=false: When printing, show all labels as the last column (default hide labels column)
      --sort-by='': If non-empty, sort list types using this field specification.  The field specification is expressed as a JSONPath expression (e.g. '{.metadata.name}'). The field in the API resource specified by this JSONPath expression must be an integer or a string.
      --template='': Template string or path to template file to use when -o=go-template, -o=go-template-file. The template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
  -w, --watch=false: After listing/getting the requested object, watch for changes. Uninitialized objects are excluded if no object name is provided.
      --watch-only=false: Watch for changes to the requested object(s), without listing/getting first.

Usage:
  kubectl get [(-o|--output=)json|yaml|wide|custom-columns=...|custom-columns-file=...|go-template=...|go-template-file=...|jsonpath=...|jsonpath-file=...] (TYPE[.VERSION][.GROUP] [NAME | -l label] | TYPE[.VERSION][.GROUP]/NAME ...) [flags] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
```

### $ kubectl version
```
Client Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.4", GitCommit:"e87da0bd6e03ec3fea7933c4b5263d151aafd07c", GitTreeState:"clean", BuildDate:"2021-02-18T16:12:00Z", GoVersion:"go1.15.8", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.2", GitCommit:"faecb196815e248d3ecfb03c680a4507229c2a56", GitTreeState:"clean", BuildDate:"2021-01-13T13:20:00Z", GoVersion:"go1.15.5", Compiler:"gc", Platform:"linux/amd64"}
```

### $ kubectl get nodes
```
$ kubectl get nodes
NAME       STATUS   ROLES                  AGE     VERSION
minikube   Ready    control-plane,master   7m15s   v1.20.2
```

### Implante nosso aplicativo
Vamos implantar nosso primeiro aplicativo no Kubernetes com o comando kubectl create deployment. Precisamos fornecer o nome da implantação e o local da imagem do aplicativo (inclua o URL completo do repositório para imagens hospedadas fora do hub do Docker).

```
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```

```
deployment.apps/kubernetes-bootcamp created
```

Great! You just deployed your first application by creating a deployment. This performed a few things for you:

```
- Procurou um nó adequado onde uma instância do aplicativo pudesse ser executada (temos apenas 1 nó disponível);
- Agendou o aplicativo para ser executado nesse Node;
- Configurou o cluster para reagendar a instância em um novo Node quando necessário.
```

Para listar suas implantações, use o comando get deployments:

```
kubectl get deployments
```

# Deploy criado

### $ kubectl get deployments
```
$ kubectl get deployments
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp   1/1     1            1           5m22s
```

Vemos que há 1 implantação executando uma única instância do seu aplicativo. A instância está sendo executada dentro de um contêiner do Docker em seu nó.

### Veja nosso aplicativo
Os pods executados no Kubernetes são executados em uma rede privada e isolada. Por padrão, eles são visíveis de outros pods e serviços dentro do mesmo cluster kubernetes, mas não fora dessa rede. Quando usamos kubectl, estamos interagindo por meio de um endpoint de API para nos comunicarmos com nosso aplicativo.

Abordaremos outras opções sobre como expor seu aplicativo fora do cluster kubernetes no Módulo 4.

O comando kubectl pode criar um proxy que encaminhará as comunicações para a rede privada de todo o cluster. O proxy pode ser encerrado pressionando control-C e não mostrará nenhuma saída enquanto estiver em execução.

Abriremos uma segunda janela de terminal para executar o proxy.

```
echo -e "\n\n\n\e[92mStarting Proxy. After starting it will not output a response. Please click the first Terminal Tab\n"; 
kubectl proxy
```

Agora temos uma conexão entre nosso host (o terminal online) e o cluster Kubernetes. O proxy permite o acesso direto à API desses terminais.

Você pode ver todas essas APIs hospedadas por meio do endpoint do proxy. Por exemplo, podemos consultar a versão diretamente pela API usando o comando curl:

```
curl http://localhost:8001/version
```

```
$ curl http://localhost:8001/version
{
  "major": "1",
  "minor": "20",
  "gitVersion": "v1.20.2",
  "gitCommit": "faecb196815e248d3ecfb03c680a4507229c2a56",
  "gitTreeState": "clean",
  "buildDate": "2021-01-13T13:20:00Z",
  "goVersion": "go1.15.5",
  "compiler": "gc",
  "platform": "linux/amd64"
}$
```

### $ curl http://localhost:8001/api/v1/nmespaces/default/pods
```
$ curl http://localhost:8001/api/v1/nmespaces/default/pods/$POD_NAME/
<a href="/api/v1/namespaces/default/pods/">Moved Permanently</a>.

namespaces/default/pods:8001/api/v1/ 
{
  "kind": "PodList",
  "apiVersion": "v1",
  "metadata": {
    "resourceVersion": "1203"
  },
  "items": [
    {
      "metadata": {
        "name": "kubernetes-bootcamp-57978f5f5d-j9rjf",
        "generateName": "kubernetes-bootcamp-57978f5f5d-",
        "namespace": "default",
        "uid": "bdc11e3e-3512-49fa-b062-8c5cc74c53aa",
        "resourceVersion": "670",
        "creationTimestamp": "2022-04-12T17:36:31Z",
        "labels": {
          "app": "kubernetes-bootcamp",
          "pod-template-hash": "57978f5f5d"
        },
        "ownerReferences": [
          {
            "apiVersion": "apps/v1",
            "kind": "ReplicaSet",
            "name": "kubernetes-bootcamp-57978f5f5d",
            "uid": "246ffdef-9380-4c96-92a2-29f0290e115a",
            "controller": true,
            "blockOwnerDeletion": true
          }
        ],
        "managedFields": [
          {
            "manager": "kube-controller-manager",
            "operation": "Update",
            "apiVersion": "v1",
            "time": "2022-04-12T17:36:31Z",
            "fieldsType": "FieldsV1",
            "fieldsV1": {"f:metadata":{"f:generateName":{},"f:labels":{".":{},"f:app":{},"f:pod-template-hash":{}},"f:ownerReferences":{".":{},"k:{\"uid\":\"246ffdef-9380-4c96-92a2-29f0290e115a\"}":{".":{},"f:apiVersion":{},"f:blockOwnerDeletion":{},"f:controller":{},"f:kind":{},"f:name":{},"f:uid":{}}}},"f:spec":{"f:containers":{"k:{\"name\":\"kubernetes-bootcamp\"}":{".":{},"f:image":{},"f:imagePullPolicy":{},"f:name":{},"f:resources":{},"f:terminationMessagePath":{},"f:terminationMessagePolicy":{}}},"f:dnsPolicy":{},"f:enableServiceLinks":{},"f:restartPolicy":{},"f:schedulerName":{},"f:securityContext":{},"f:terminationGracePeriodSeconds":{}}}
          },
          {
            "manager": "kubelet",
            "operation": "Update",
            "apiVersion": "v1",
            "time": "2022-04-12T17:36:32Z",
            "fieldsType": "FieldsV1",
            "fieldsV1": {"f:status":{"f:conditions":{"k:{\"type\":\"ContainersReady\"}":{".":{},"f:lastProbeTime":{},"f:lastTransitionTime":{},"f:status":{},"f:type":{}},"k:{\"type\":\"Initialized\"}":{".":{},"f:lastProbeTime":{},"f:lastTransitionTime":{},"f:status":{},"f:type":{}},"k:{\"type\":\"Ready\"}":{".":{},"f:lastProbeTime":{},"f:lastTransitionTime":{},"f:status":{},"f:type":{}}},"f:containerStatuses":{},"f:hostIP":{},"f:phase":{},"f:podIP":{},"f:podIPs":{".":{},"k:{\"ip\":\"172.18.0.6\"}":{".":{},"f:ip":{}}},"f:startTime":{}}}
          }
        ]
      },
      "spec": {
        "volumes": [
          {
            "name": "default-token-x9x9q",
            "secret": {
              "secretName": "default-token-x9x9q",
              "defaultMode": 420
            }
          }
        ],
        "containers": [
          {
            "name": "kubernetes-bootcamp",
            "image": "gcr.io/google-samples/kubernetes-bootcamp:v1",
            "resources": {
              
            },
            "volumeMounts": [
              {
                "name": "default-token-x9x9q",
                "readOnly": true,
                "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount"
              }
            ],
            "terminationMessagePath": "/dev/termination-log",
            "terminationMessagePolicy": "File",
            "imagePullPolicy": "IfNotPresent"
          }
        ],
        "restartPolicy": "Always",
        "terminationGracePeriodSeconds": 30,
        "dnsPolicy": "ClusterFirst",
        "serviceAccountName": "default",
        "serviceAccount": "default",
        "nodeName": "minikube",
        "securityContext": {
          
        },
        "schedulerName": "default-scheduler",
        "tolerations": [
          {
            "key": "node.kubernetes.io/not-ready",
            "operator": "Exists",
            "effect": "NoExecute",
            "tolerationSeconds": 300
          },
          {
            "key": "node.kubernetes.io/unreachable",
            "operator": "Exists",
            "effect": "NoExecute",
            "tolerationSeconds": 300
          }
        ],
        "priority": 0,
        "enableServiceLinks": true,
        "preemptionPolicy": "PreemptLowerPriority"
      },
      "status": {
        "phase": "Running",
        "conditions": [
          {
            "type": "Initialized",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2022-04-12T17:36:31Z"
          },
          {
            "type": "Ready",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2022-04-12T17:36:32Z"
          },
          {
            "type": "ContainersReady",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2022-04-12T17:36:32Z"
          },
          {
            "type": "PodScheduled",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2022-04-12T17:36:31Z"
          }
        ],
        "hostIP": "10.0.0.28",
        "podIP": "172.18.0.6",
        "podIPs": [
          {
            "ip": "172.18.0.6"
          }
        ],
        "startTime": "2022-04-12T17:36:31Z",
        "containerStatuses": [
          {
            "name": "kubernetes-bootcamp",
            "state": {
              "running": {
                "startedAt": "2022-04-12T17:36:32Z"
              }
            },
            "lastState": {
              
            },
            "ready": true,
            "restartCount": 0,
            "image": "jocatalin/kubernetes-bootcamp:v1",
            "imageID": "docker-pullable://jocatalin/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af",
            "containerID": "docker://775b471ea19984c7ce9f79fb6f6bb381f48d55a1298d646ec4959fe3920b0e49",
            "started": true
          }
        ],
        "qosClass": "BestEffort"
      }
    }
  ]
$ 
```

Nota: Verifique a parte superior do terminal. O proxy foi executado em uma nova aba (Terminal 2), e os comandos recentes foram executados na aba original (Terminal 1). O proxy ainda é executado na segunda guia, e isso permitiu que nosso comando curl funcionasse usando localhost:8001.

**Obeservação**
Se a porta 8001 não estiver acessível, verifique se o proxy kubectl iniciado acima está em execução.

O servidor de API criará automaticamente um endpoint para cada pod, com base no nome do pod, que também pode ser acessado por meio do proxy.

Primeiro precisamos pegar o nome do Pod, e vamos armazenar na variável de ambiente POD_NAME:

```
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
```

Você pode acessar o pod por meio da API executando:

```
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/
```

# Visualizando Pods e Nós (Nodes)

### Objetivos
- Aprenda sobre Pods do Kubernetes;
- Aprenda sobre Nós do Kubernetes;
- Solucionar problemas de aplicativos implantados no Kubernetes.


### Kubernetes Pods

Quando você criou um Deployment no Módulo 2, o Kubernetes criou um Pod para hospedar a instância do seu aplicativo. Um Pod é uma abstração do Kubernetes que representa um grupo de um ou mais contêineres de aplicativos (como Docker) e alguns recursos compartilhados para esses contêineres. Esses recursos incluem:

- Armazenamento compartilhado, como Volumes;
- Rede, como um endereço IP único no cluster;
- Informações sobre como executar cada contêiner, como a versão da imagem do contêiner ou portas específicas a serem usadas.


Um Pod define um "host lógico" específico para o aplicativo e pode conter diferentes contêineres que, na maioria dos casos, são fortemente acoplados. Por exemplo, um Pod pode incluir o contêiner com seu aplicativo Node.js, bem como um outro contêiner que alimenta os dados a serem publicados pelo servidor web Node.js. Os contêineres de um Pod compartilham um endereço IP e intervalo de portas; são sempre localizados, programados e executam em um contexto compartilhado no mesmo Nó.

Pods são a unidade atômica na plataforma Kubernetes. Quando criamos um Deployment no Kubernetes, esse Deployment cria Pods com contêineres dentro dele (em vez de você criar contêineres diretamente). Cada Pod está vinculado ao nó onde está programado (scheduled) e lá permanece até o encerramento (de acordo com a política de reinicialização) ou exclusão. Em caso de falha do nó, Pods idênticos são programados em outros nós disponíveis no cluster.

### Visão geral sobre os Pods

<img src="
