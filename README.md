# Kubernetes

## Lista clusters
- kubectx

## Lista namespaces de um cluster
- kubens

## Pods com estado evicted (falha)
- kubectl get pods --all-namespaces --field-selector 'status.phase==Failed'

## aumentar quantidade de pods
- kubectl scale --current-replicas=6 --replicas=8 deployment -l app=production

## deleta pods, services, deployments por label app=review-envs-graph-i9cdkl
- kubectl delete pods,services,deployments -l app={_text-used-in-app-label_}

## pega do pod a informação que está na linha IP:
- kubectl describe pod -l role={_role-name_} | grep IP\:\ | sed "s/IP:/ /g" | tr -d '[:space:]'

## Detalha o POD pelo nome
- kubectl describe pod {_pod-name_}

## Listando HPA
- kubectl get hpa

## Buscar informação da linha com texto "Node:", em todos os pods
- kubectl describe pod | grep Node:

## Buscar informação da linha com texto "Labels:" em um pod
- kubectl describe pod {_pod-name_} | grep Node:

## Acessa bash no pod (para instalar no pod, usamos APK ADD...)
- kubectl exec -it {_pod-name_} -- /bin/sh

## Encaminhamento de porta local para um container
- kubectl port-forward {_pod-name_} {_local-port_}:{_container-port_}

## Log do pod e com grep de 10 linhas
- kubectl logs {_pod-name_} --follow=true
- kubectl logs {_pod-name_} | grep -A 10 80409001331404
- kubectl logs {_pod-name_} --follow=true | grep CT000409
- kubectl logs -l app={_app-label_} -f | grep 33817526857

## Listar nodes
- kubectl get nodes

## Cordon (antes do drain)
for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=default-pool -o=name); do
  kubectl cordon "$node";
done

## remover/deletar uma node do cluster
- kubectl delete node {_node-name_}

## drenar os pods de um node
for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=default-pool -o=name); do
  kubectl drain --force --ignore-daemonsets --delete-local-data --grace-period=10 "$node";
done

- kubectl drain {_node-name_} --ignore-daemonsets --delete-local-data

## Deletar node pool
- gcloud container node-pools delete {_node-name-pool_} --cluster {_cluster-name_}

## Para listar replicasets
- kubectl get rs

## Para listar deployments
- kubectl get deployments

## Para auto escalar um deployment
- kubectl autoscale deployment {_deployment-name_} --min=1 --max=3 --cpu-percent=85

## Para escalar um deployment
- kubectl scale deployment {_deployment-name_} --replicas=10

### Ref: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-strong-getting-started-strong-

# Docker

## Criar uma imagem docker com nome xpto
- sudo docker build -t xpto .

## Cria uma instancia docker a partir de uma image com nome xpto e expoe a porta 5000 pela 3333
- sudo docker run -d -p 3333:5000 xpto

## Remove uma imagem docker com nome xpto
- sudo docker image rm xpto -f

## Remove um container com id aaa111
- sudo docker container rm aaa111

## Para a execução de um container com id aaa111
- sudo docker container stop aaa111

## Remove todos containers
- sudo docker rm $(sudo docker ps -a -q)

## Remove todas imagens
- sudo docker rmi $(sudo docker images -q)
