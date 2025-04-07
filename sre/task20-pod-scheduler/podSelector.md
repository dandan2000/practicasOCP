Asignar los Pods de un Deployment a un Nodo con cierta/s label/s

Esto es mandatorio, se asigna al nodo que cumple la condición o no se asignada para nada.
El label asignado al nodo debe coincidir con el asignado a deployment.

talos % kubectl get node -l node-role.kubernetes.io/control-plane!=  --show-labels
NAME            STATUS   ROLES    AGE   VERSION   LABELS
talos-i15-fgj   Ready    <none>   74m   v1.32.3   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=talos-i15-fgj,kubernetes.io/os=linux,propiedad=primary
talos-sco-25q   Ready    <none>   22h   v1.32.3   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=talos-sco-25q,kubernetes.io/os=linux,propiedad=secondary

En los nodos se asigno una etiqueta propiedad con el valor primary y secondary.

oc label node/talos-i15-fgj propiedad=primary
oc label node talos-sco-25q propiedad=secondary

kind: Deployment
...
  template:
    metadata:
    spec:
      containers:
      dnsPolicy: ClusterFirst
      nodeSelector:
        propiedad: secondary

## Es decir que se coloca dentro del template del pod, en la spec a nivel de los tags que afectan a todos los contendedores, visualmente mismo nieel que containers:


También se podría asignar a un nodo por el nombre usando el label default kubernetes.io/hostname=<node-name>

Node label:
    kubernetes.io/hostname=talos-i15-fgj

Deployment tag:

      nodeSelector:
        propiedad: talos-i15-fgj

También podría tener que cumplir varias condiciones:


Node label:
    consumo: alto
    almacenamiento: amplio

Deployment tag:

  nodeSelector:
    consumo: alto
    almacenamiento: amplio

