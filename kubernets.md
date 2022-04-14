```sh
kubectl get nodes
kubectl get nodes -o wide
kubectl get configmap
kubectl run nginx-pod --image=nginx:latest
kubectl get pods --watch
kubectl describe pod nginx-pod
kubectl edit pod nginx-pod
kubectl apply -f .\primeiro-pod.yaml
kubectl delete pod nginx-pod
kubectl delete -f .\primeiro-pod.yaml
kubectl exec -it portal-noticias -- bash
kubectl exec -it portal-noticias --container container_name -- bash
```

SVC:
pod    -> svc:          -ClusterIP  -> pod
ip        ip fixo       -NodePort      ip variavel
variavel  balanceamento -LoadBalancer

  ClusterIP: permite pods acessarem estaticamente pods

  NodePort: Abre comunicação para o mundo externo, tambem funciona como ClusterIP

  LoadBalancer: Abre comunicação para o mundo externo usanod o Load Balancer do cloud provider

ConfigMap:
Torna os pods mais portaveis, armazenando configurações. Permite Reutilização e desacoplamento.

ReplicaSet:
Pode encapsular um ou mais pods. Cria um novo pod em caso de falha.

Deployment:
Uma camada extra em cima de um replicaset para realizar controle de versão.
```sh
kubectl rollout history deployment nginx-deployment
kubectl apply -f nginx-deployment.yaml --record
kubectl annotate deployment nginx-deployment kubernets.io/change-cause="Mudança feita no deployment do nginx"
kubectl rollout undo deployment nginx-deployment --to-revision=2 #volta o deployment para a versão 2
```

Volumes:
Persistem dados dos pods. Tem ciclos de vida independentes dos containers. Porém são dependentes dos pods.
  GCP: criar disco na cloud. 
  Criar um gcePersistentVolume como no exemplo pv.yaml.
  PersistentVolume: não tem o ciclo vinculado ao pod.
  PersistentVolumeClaim: é vinculado ao PV. É usado para fazer a comunicação entre o pod e o PV.
  StorageClass: gerencia os discos e volumes dinamicamente 

Stateful Set: faz deploy de container e volumes(com replicas)

LivenessProbe: usado pelo kubelet para saber quando reiniciar um container.

ReadinessProbe: usado para saber quando o container está pronto

StartupProbes: Algumas aplicações legadas exigem tempo adicional para inicializar na primeira vez.  Nem sempre Liveness ou Readiness Probes vão conseguir resolver de maneira simples os problemas de inicialização de aplicações legadas.

HorizontalPodAutoscaler: escala automaticamente o numero de pods em um deployment, replicaset, statefulSet etc. Baseado na CPU por padrão (pode-se ser usadas outras metricas)

--kubelet-insecure-tls