<img style="float: right;" align="right" src="../../images/1546903-200.png" alt="Big Data" width="200"/>

# INF8200 - Spark 2/3 - Cluster et Jupyter

## Lignes de commande

Dans les exercices qui viennent (ainsi que dans votre future vie professionnelle !), vous n'aurez pas le choix que d'utiliser des lignes de commande. Voici notamment un tutoriel pour vous y former : [Introduction to the command-line interface](https://tutorial.djangogirls.org/en/intro_to_command_line/).

## Jupyter & PySpark avec Docker

- Déployer Jupyter en suivant les instructions suivantes : [Setting Up a PySpark Notebook using Docker: A Step-by-Step Guide](https://datascience.fm/setting-up-a-pyspark-notebook-using-docker/)

- [ ] :warning: Avant toutes autres choses, récupérez l'image _all-spark-notebook:spark-3.5.0_ (opération qui prend du temps).

- [ ] :question: Pourquoi cette approche n'est pas possible (en ce jour du 3 février 2024) avec la version 3.4.2 de Spark ?

    https://hub.docker.com/r/jupyter/all-spark-notebook/tags?page=1&name=spark

- [ ] Faire ce déploiement dans le répertoire ./docker-compose-way (cela sera utile par après). 

- [ ] De plus, déposer le fichier "googleplaystore_user_reviews.csv" dans le répertoire ./docker-compose-way/data/ (et adapter le code en conséquence)

- [ ] Utiliser un _path_ relatif au sein de la commande ```docker run -d -P --name notebook -v /Users/datasciencefm/workspace/sparkdata:/sparkdata  jupyter/all-spark-notebook:spark-3.5.0```.

    Pour ceux qui sont sous Windows: ```docker run -d -P --name notebook -v $PWD\data:/data  jupyter/all-spark-notebook:spark-3.5.0```

- [ ] :question: À quoi correspondent les paramètres -d et -P dans la commande ```docker run -d -P``` ?

    Réponse de _Copilot Chat_ :
    
        Dans la commande `docker run -d -P`, les paramètres `-d` et `-P` ont les significations suivantes :

        - `-d` : Ce paramètre fait démarrer le conteneur en mode détaché (detached mode). Cela signifie que le conteneur s'exécute en arrière-plan et ne reçoit pas les entrées du terminal. Vous obtenez le contrôle de votre terminal ou de votre invite de commande immédiatement après avoir lancé le conteneur.

        - `-P` : Ce paramètre publie tous les ports exposés du conteneur vers des ports aléatoires sur l'hôte. Cela signifie que Docker mappera tous les ports exposés à des ports aléatoires de l'hôte, permettant ainsi l'accès au conteneur via le réseau.

- [ ] Contrairement à ce qui est indiqué, vous aurez peut-être à regarder une ligne plus haut dans le _log_ : ```docker logs --tail 4 notebook``` (notez le 4 à la place du 3).

- [ ] Une fois les calculs PySpark terminés, supprimez le conteneur Jupyter.

## Déployer un _cluster_ Spark en utilisant ```Docker-compose```.

### Cluster Spark

Compléter le tutoriel [Deploy Spark using Docker-compose](https://medium.com/%2540SaphE/testing-apache-spark-locally-docker-compose-and-kubernetes-deployment-94d35a54f222) en suivant les instructions qui suivent.

- [ ] Pour éviter par après l'erreur ```pyspark.errors.exceptions.base.PySparkRuntimeError: [PYTHON_VERSION_MISMATCH] Python in worker has different version (3, 7) than that in driver 3.11, PySpark cannot run with different minor versions```, il convient d'utiliser les mêmes versions de PySpark et de Python. Pour cela, on va changer la version Python de l'image ```our-own-apache-spark:3.5.0``` que l'on va créer :
    - la version mineure que l'on recherche est données par la commande ```$ docker run jupyter/all-spark-notebook:spark-3.5.0 python --version```
    - pour se faire, utiliser le fichier [Dockerfile-apache-spark.3.5.0](./Dockerfile-apache-spark.3.5.0).

- [ ] Si un port (7000 par exemple) est déjà utilisé (_Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:7000 -> 0.0.0.0:0: listen tcp 0.0.0.0:7000: bind: address already in use_), que faire ?

- [ ] "_Now check that spark is running by accessing the ui https://localhost:4040_" ne sera possible qu'à la dernière étape de ce cours.

- [ ] Automatiser l'extraction du _Container ID_ utilisé par ```docker exec```

    Aide (fournie par [Gihub Copilot](https://github.com/features/copilot) Chat, lequel est gratuit pour les étudiants) :

        Refine the following code by automating the extraction of the container ID to be used by docker exec: To get all running containers, run docker ps The one running the driver is prefixed by docker-spark-master. Get the Container Id (in my case 78a6fbac7acf) and get into it docker exec -i -t 78a6fbac7acf /bin/bash
        -------------
        You can automate the extraction of the container ID using the docker ps command with a filter and awk to parse the output. Here's how you can do it:

        # Get the container ID of the running spark master
        container_id=$(docker ps --filter "ancestor=docker-spark-master" --format "{{.ID}}")

        # Use the container ID to execute a command
        docker exec -i -t $container_id /bin/bash

        This script first runs docker ps with a filter to only show containers based on the docker-spark-master image. The --format "{{.ID}}" option tells Docker to only output the container IDs. The output (the container ID) is then stored in the container_id variable.

        The docker exec command then uses this container_id variable. If there are multiple containers running the spark master, this will take the ID of the first one. If you want to enter all of them, you would need to loop over the IDs.

- [ ] Ouvrir la console Spark Master, exposée en interne sur le port 8080

- [ ] Ne pas oublier de nettoyer les conteneurs créé par docker compose up
    ```shell
    $ docker compose down
    ```

    :bulb: Pour sortir d'un ```docker exec```, utiliser la commande ```exit```.

    :bulb: Pour sortir d'un ```docker-compose up```, ```press Ctrl+C```

### Combiner le cluster Spark avec Jupyter

- [ ] Ajouter Jupyter tel que vu précédemment au _cluster_ Spark à travers _docker-compose_

    - [ ] S'inspirer de : [Jupyter notebook development workspace using Docker, Docker Compose and Git](https://nezhar.com/blog/jupyter-notebook-development-workspace-using-docker-and-git/)

    - [ ] Faire en sorte que le port 4040 du conteneur Jupyter soit bien accessible (en complément du port 8888).

    - [ ] Le répertoire ```work``` devra être accessible depuis le conteneur Docker "notebook".

    - [ ] Votre notebook devra être sauvegardé dans ce même répertoire ```work```.

    - [ ] Lancer les mêmes calculs utilisant "googleplaystore_user_reviews.csv" que précédemment.

        - [ ] Vérifier la console ```Spark Master``` lors des étapes qui suivent
        - [ ] Lancer le calcul en mode "local" pour vérifier que le Jupyter est bien configuré
        - [ ] Relancer ("restart the kernel") le même calcul à travers le cluster Spark
            - L'adresse du cluster peut être trouvée dans le log de docker-compose
            - le fichier ".csv" doit bien être "monté" dans les volumes ./data:/opt/spark-data des _workers_
        - [ ] Allez à l'adresse http://localhost:4040/jobs/ pour parcourir la console Spark (laquelle est fournie à travers la session Spark du client, i.e. Jupyter).

- [ ] Une fois ces opérations terminées, ne pas oublier de retirer la configuration ```docker-compose```.

    - [ ] :question: Est-ce que le notebook que vous avez préalablement créé sera alors détruit ? Pourquoi ?

## Solution complète

Si vous avez terminé tous les exercices et que vous voulez valider ce que vous avez fait, ou si vous êtes **bloqués** lors d'une étape, vous pouvez regarder une solution complète : [cluster_jupyter.md](cluster_jupyter.md).

- :warning: Les commandes et fichiers utilisés auront peut-être à être adaptées à votre propre environnement (Windows vs Linux, amd64 vs arm64, etc.).
- :bulb: D'aller directement à la solution proposée sans chercher par vous-même à résoudre les difficultés rencontrées ne vous apprendra au final pas grand chose...

<img style="float: right;" align="right" src="../../images/uqam.png" alt="Big Data" width="200"/>

# Copyright

Ce cours est préparé et donné conjointement par Jean-François Rajotte et Laurent Magnin - © UQÀM 2024