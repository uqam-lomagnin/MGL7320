<img style="float: right;" src="../../images/component_engineering.svg" alt="EngineeringAISystems" width="250"/>

## MGL7320 - Ingénierie logicielle des systèmes d'IA
# 03 - Docker et autres outils

## Prelude

- Quizz - https://ahaslides.com/IZN44
- Questions / réponses concernant le cours de la semaine dernière

## Théorie
- :book: [Docker & Kubernetes - aka K8s](<inf8200_cours3 - docker.pdf>)

## Pratique

- <details>
  <summary>Références utiles</summary>

    * https://docs.docker.com/engine/reference/commandline/history/
    * https://docs.docker.com/engine/reference/commandline/exec/
    * https://www.cyberciti.biz/faq/bash-infinite-loop/
    * https://www.rapidtables.com/code/linux/ls.html
    </details>

- [Tutoriel Docker 101](https://www.docker.com/101-tutorial/)
    - ```docker run -dp 80:80 docker/getting-started``` (Intel CPU)
    - ```docker run --platform linux/amd64 -dp 80:80 docker/getting-started``` (Mac Silicon CPU)
    - Ouvrir [http://localhost](http://localhost) dans votre navigateur et suivre les instructions.

- Refaire par vous-même l'exercice [Hadoop de la semaine dernière](../hadoop/hadoop_wordcount/python_mapreduce.md).

- [Tutoriel Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/) (alternative : [Mini tutoriel Kubernetes](./mini_tutoriel_k8s.md))

    Un _cluster_ K8s est fourni par Docker Desktop (en version single-node). Inutile donc d'installer ici _Minikube_. Par contre, il est indispensable d'[installer et configurer kubectl](https://docs.docker.com/desktop/kubernetes/).

:warning: Ne pas oublier en fin de session de "nettoyer" & de quitter Docker !
- [How to Do a Clean Restart of a Docker Instance](https://docs.tibco.com/pub/mash-local/4.3.0/doc/html/docker/GUID-BD850566-5B79-4915-987E-430FC38DAAE4.html)


## Travail personnel pour la semaine prochaine
- [x] (**optionnel**) Compléter le [Tutoriel Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/) (alternative : [Mini tutoriel Kubernetes](./mini_tutoriel_k8s.md))

    Un _cluster_ K8s est fourni par Docker Desktop (en version single-node). Inutile donc d'installer ici _Minikube_. Par contre, il est indispensable d'[installer et configurer kubectl](https://docs.docker.com/desktop/kubernetes/).

    :warning: Ne pas oublier en fin de session de "nettoyer" & de quitter K8s !
    - [kubernetes cleanup of pods,service,deployment etc](https://stackoverflow.com/questions/57014430/kubernetes-cleanup-of-pods-service-deployment-etc)
    - Vous pouvez également appliquer un "_reset_" complet :
![reset_k8s.png](reset_k8s.png)

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2023-2024