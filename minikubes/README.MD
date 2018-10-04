# Minikube
Minikube ist ein Tool, dass es einem erlaubt Kubernetes lokal auszuführen. Auf Minikube läuft ein Single-Node Kubernetes Cluster.

## Vorraussetzungen

Virtuelle Maschine (ubuntu)  
Docker auf Ubuntu  
Installation von Kuberenetes [1]  
Installation von Minikubes [2]  

### Installation Kubernetes
Um Minikube zu installieren muss vorher kubectl installiert werden.  
Dieses funktioniert aber wiederrum nur dann, wenn auch minikube installiert wurde. Also werden folgende Schritte in dieser Reihenfolge durchgeführt:

    sudo snap install kubectl --classic

### Installation Docker
Um Minikube zu starten benötigt man außerdem Docker:

    sudo apt-get install docker.io
### Installation Minikube
Mit diesem Befehl wird Minikube automatisch installiert, sofern es noch nicht da ist.  
Mit der Option "--vm-driver=none" kann man sagen, dass in unserem Fall Docker verwendet werden soll.

    sudo minikube start --vm-driver=none
Man kann die Installation verifizieren in dem man folgendes schreibt und kein Fehler im Output angezeigt wird:

    sudo kubectl version

## Aufgabendurchführung
Nun werden die Schritte aus dem Kubernet101[3] auch hier ausgeführt.  
Die durchgeführten Schritte sind im Main-README ersichtlich.

## Quellen
[1]https://kubernetes.io/docs/tasks/tools/install-kubectl/  
[2]https://kubernetes.io/docs/tasks/tools/install-minikube/  
[3]https://kubernetes.io/docs/tutorials/k8s101/
