# syt5-gk923-cloud-computing-kubernetes-mfentler-tgm
syt5-gk923-cloud-computing-kubernetes-mfentler-tgm created by GitHub Classroom

## Prerequisits
Man benötigt einen Kubernetes Cluster und das kubectl Command-Line Tool.

## Pods
Ein Pod eschreibt das Deployment von einem oder mehreren Container. Der Pod beschreibt einen gewünschten Stand des Systems. Kubernetes achtet darauf, dass die Container dann auch immer den gewünschten Status haben.  

Pod erstellen und alle bestehenden auflisten: 

    kubectl create -f https://k8s.io/examples/pods/simple-pod.yaml
    kubectl get pods
Die IP des Pods ist meistens nicht von außen erreichbar.  
Workaround: Busy-Box erstellen

    kubectl run busybox --image=busybox --restart=Never --tty -i --generator=run-pod/v1 --env "POD_IP=$(kubectl get pod nginx -o go-  template='{{.status.podIP}}')"
    u@busybox$ wget -qO- http://$POD_IP # Run in the busybox container
    u@busybox$ exit # Exit the busybox container
Busybox wieder löschen:

    kubectl delete pod busybox
## Volumes
Um einen persitenten Speicher zu erstellen werden Volumes(Speicherplätze) verwendet.  
Volume definieren:

    volumes:
    - name: redis-storage
      emptyDir: {}
Volume Mount mit container definition:

    volumeMounts:
    # name must match the volume name defined in volumes
    - name: redis-storage
      # mount path within the container
      mountPath: /data/redis
#### Beispiel
Redis pod (redis.yaml)

    touch redis.yaml
    
    
    apiVersion: v1
    kind: Pod
    metadata:
      name: redis
    spec:
      containers:
      - name: redis
        image: redis
        volumeMounts:
        - name: redis-storage
          mountPath: /data/redis
      volumes:
      - name: redis-storage
        emptyDir: {}
### Arten von Volumes
 - EmptyDir:  
 Erstellt ein neues directory das nur solange existiert, solange der Pod rennt.
 - HostPath:  
 Mountet ein bestehendes Filesystem auf dem node-Filesystem.

## Multiple Containers
Es gibt oft Anwendungen bei denen mehrerer Container zusammen arbeiten sollen. Ein Beispiel kann eine Webseite sein, die einen Git-Hub Helperjob implementiert.  
Was man in diesem Beispiel noch sehen kann, ist das beide Container das Volume "www-data" mounten, der Webserver darauf aber nur readonly Zugriffsrechte hat. 

    apiVersion: v1
    kind: Pod
    metadata:
      name: www
    spec:
      containers:
      - name: nginx
        image: nginx
        volumeMounts:
        - mountPath: /srv/www
          name: www-data
          readOnly: true
      - name: git-monitor
        image: kubernetes/git-monitor
        env:
        - name: GIT_REPO
          value: http://github.com/some/repo.git
        volumeMounts:
        - mountPath: /data
          name: www-data
      volumes:
      - name: www-data
        emptyDir: {}
