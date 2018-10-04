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
### Installation & Start von Minikube
Mit diesem Befehl wird Minikube automatisch installiert, sofern es noch nicht da ist.  
Mit der Option "--vm-driver=none" kann man sagen, dass in unserem Fall Docker verwendet werden soll um Minikube zu starten.

    curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.29.0/minikube-linux-amd64 && chmod +x minikube && sudo cp minikube /usr/local/bin/ && rm minikube
    sudo minikube start --vm-driver=none
Man kann die Installation verifizieren in dem man folgendes schreibt und kein Fehler im Output angezeigt wird:

    sudo kubectl version

## Aufgabendurchführung
In diesem Beispiel wird nun ein Echoserver-Pod erstellt:

    sudo kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.10 --port=8080
Anschließend muss man den Pod exposen

    sudo kubectl expose deployment hello-minikube --type=NodePort
Somit hat man nun den Pod erstellt. Dieser braucht allerdings ein paar Minuten bis er fertig ist. Überprüfen kann man das mit dem Command:

    sudo kubectl get pod
Da sollte dann bei dem Status des Pods "Running" stehen.  
Jetzt kann man curlen:

    sudo curl $(sudo minikube service hello-minikube --url)

## Quellen
[1]https://kubernetes.io/docs/tasks/tools/install-kubectl/  
[2]https://kubernetes.io/docs/tasks/tools/install-minikube/  
[3]https://kubernetes.io/docs/tutorials/k8s101/  
[4]https://kubernetes.io/docs/setup/minikube/
