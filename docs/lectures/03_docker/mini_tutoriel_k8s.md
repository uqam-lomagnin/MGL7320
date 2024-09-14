# Mini tutoriel Kubernetes

Cette démo est une version réduite du tutoriel Kubernetes [Deploy an App](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/).
Le but est de présenter l'option de mise à l'échelle (augmenter et réduire) d'un cluster.
Il est attendu que kubernetes est activé sur Docker Desktop (instructions [ici](https://docs.docker.com/desktop/kubernetes/)).

## Déployer sa première APP
Une fois Kubernetes activé sur Docker Desktop, nous avons un cluster Kubernetes composé d'un noeud.
Pour vérifier que Kubernetes est bien activé, on s'assure que l'outil de l'interface de commande [kubectl](https://kubernetes.io/docs/reference/kubectl/) est disponible.
Par exemple, en vérifiant la version:

```kubectl version```


Ensuite on peut créer un déploiement avec la commande suivante:

```kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1```

Le cluster a un réseau privé, pour pouvoir y accéder il faut créer un proxy (dans une fenêtre du terminal dédiée)

```kubectl proxy```


cette commande devrait retourner le message suivant:

```Starting to serve on 127.0.0.1:8001```


On peut alors communiquer avec le cluster par le port `8001`, on peut vérifier avec la commande suivante:

```curl http://localhost:8001/version```



### [Exploration des noeuds et des pods](https://kubernetes.io/docs/tutorials/kubernetes-basics/explore/explore-intro/)
Les cluster sont composés de **noeuds** (nodes), soit des VM ou des serveurs physique, contenant des **pods**.
Les pods sont les éléments de base sur une plateforme kubernetes. Un déploiement crée des pods contenant des conteneurs.
Un pod est associé à un noeud et si ce noeud tombe en panne, le pod est recréé sur un autre noeud du cluster par le “Kubernetes Deployment controller”.


On observe les pods du cluster avec la commande suivante:

```kubectl get pods```

En utilisant le nom du pod retourné par cette commande, on peut démarrer une session interactive dans le pod.

```kubectl exec -ti <nom du pod> -- bash```


Dans la session, on peut explorer le code de l’application

```cat server.js```

Pour quitter la sesssion sur le pod, il suffit de taper
```exit```

### [Services](https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/)

Les services sont des abstractions definissant un ensemble de Pods et leur politique d’acces.
Un service est créé par defaut au demarrage de notre cluster, nous pouvons le voir avec la commande suivante

```kubectl get services```

On peut aussi l’inspecter avec la commande suivante

```kubectl describe services/kubernetes```


On peut aussi creer un autre service exposé au trafic externe en lui donnant le type “NodePort” :

```kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080```

On peut voir les information sur le service:

```kubectl describe services/kubernetes-bootcamp```

Pour confirmer que le service est bien exposé à l’exterieur, on peut contacter le port (obtenu de la commande précédente) avec la commande *curl*:

```curl http://"$(minikube ip):$NODE_PORT"```

 avec le cluster de Docker Desktop, le `minikube ip`` est `localhost``.

### [Mise à l’echelle](https://kubernetes.io/docs/tutorials/kubernetes-basics/scale/scale-intro/)


La commande `kubectl scale` permet d’augmenter le nombre de copies d’un pod.

Utilité:
* Partager la demande (load balance)
* Mettre à jour le service sans “downtime”


Voyons d’abord le nombre de replicas

```kubectl get rs```

Il devrait y avoir un seul replica, augmentons ce nombre a 4 

```kubectl scale deployments/kubernetes-bootcamp --replicas=4```

On peut voir le nombre de réplicas augmentà en reprenant la commande

```kubectl get rs```



#### Partage de la demande (load balancing)

On s’assure qu’on a le le service qui fonctionne

```kubectl describe services/kubernetes-bootcamp```


On contact le service par son port, comme avant, plusieurs fois
```curl http://"$(minikube ip):$NODE_PORT"```

On constate, qu'on a des service traité par des pods différents, voici des exemples de réponses:

* ```Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-855d5cc575-p85n2 | v=1```
* ```Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-855d5cc575-pnk75 | v=1```
* ...

Les chaînes de caractères différentes (à la fin de `kubernetes-bootcamp-`) à chaque appel correspondent à des pods différents.
La charge des appels est donc partagée entre les 4 réplicas du service.


Enfin, on peut aussi diminuer l’echelle avec la même commande:
```kubectl scale deployments/kubernetes-bootcamp --replicas=2```

### Enlever tout Kubernetes

Enfin pour arrêter le cluster, vous pouver utiliser les commande suivantes (voir [ici](https://stackoverflow.com/a/57017889/4615292)):

```
kubectl delete deployments --all
kubectl delete services --all
kubectl delete pods --all
kubectl delete daemonset --all
```