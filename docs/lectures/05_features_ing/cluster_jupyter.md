# Spark avec un docker

## Pyspark avec Jupyter

Instructions:
* [original](https://datascience.fm/setting-up-a-pyspark-notebook-using-docker/)
* [github du cours](./spark.md)

On obtient l'image docker de la version de Spark voulue

```docker pull jupyter/all-spark-notebook:spark-3.5.0```

A partir de votre répertoire de travail, on créé les sous-répertoires suggéré en y mettant les données

```mkdir -p docker-compose-way/data```

mettre le fichier `googleplaystore_user_reviews.csv` dans le repertoire ci-haut.


On démarre le conteneur

```docker run -d -P --name notebook -v ./data:/data  jupyter/all-spark-notebook:spark-3.5.0```

Pour les utilisateurs **Windows**, la commande est

```docker run -d -P --name notebook -v $PWD\data:/data  jupyter/all-spark-notebook:spark-3.5.0```


Trouvons le mapping entre le port 8888 interne (le port part [defaut](https://docs.jupyter.org/en/latest/running.html#how-do-i-start-the-notebook-using-a-custom-ip-or-port) du serveur Jupyter) et le port externe de notre conteneur `notebook`.

```docker port notebook 8888```

Exemple de réponse `0.0.0.0:32778`


On a ensuite besoin du *token* pour acceder au notebook, on peut le trouver en regardant la reponse de la commande suivante:

```docker logs --tail 4 notebook```

Le token est une longue chaîne de caractères suivant `lab?token=`.
Avec le port et le *tokem*, on peut acceder au notebook avec l'url suivant (en changeant le port et le *token* par celui que vous avez trouvé):

```http://127.0.0.1:32778/lab?token=74e5a21306b94eb8902b674e373cfb3bf86847f6f6171941```


Dans le notebook, il faudra modifier la variable `csv_path` ainsi

```csv_path = "/data/googleplaystore_user_reviews.csv"```

Vous pouvez essayer les commandes du tutoriel [original](https://datascience.fm/setting-up-a-pyspark-notebook-using-docker/).

Une fois terminée, on efface le notebook

```docker rm -f notebook```


## Deployer un cluster Spark avec Docker-compose

Blog original [ici](https://medium.com/@SaphE/testing-apache-spark-locally-docker-compose-and-kubernetes-deployment-94d35a54f222)

Il faut prendre une autre version de python et Spark, alors on prends un autre [Dockerfile](https://github.com/uqam-inf8200/hiver-2024/blob/main/lectures/spark/Dockerfile-apache-spark.3.5.0).

On crée le fichier (pas de modification du blog original) 

```start-spark.sh```


On construit l'image de base de notre cluster (cette commande prend environ 10 minutes)
```docker build -t our-own-apache-spark:3.5.0 .```

On crée le ficher `docker-compose.yml` tel que suggère dans le blog original en modifiant le nom de l'image `our-own-apache-spark:3.5.0` par `docker build -t our-own-apache-spark:3.5.0` créé a l'etape précédente.


On démarre le cluster

```docker-compose up```

On peut voir le sparkUI au URL suivant (contrairement au port 4040 suggéré par le blog): `http://127.0.0.1:9090`

### Envoyer un job

Il faut d'abord se connecter sur le driver, lequel se trouve sur le conteneur master.
On trouve le conteneur master avec la commande suivante

```docker ps```

L'ID du conteneur master a le string `master` dans son nom

```docker exec -i -t docker-compose-way-spark-master-1 /bin/bash```


Nous somme maintenant dans console `bash` du conteneur master.
A partir du répertoire bin, on envoie un job exemple.

```cd bin```

```./spark-submit --master spark://0.0.0.0:7077 --name spark-pi --class org.apache.spark.examples.SparkPi  local:///opt/spark/examples/jars/spark-examples_2.12-3.5.0.jar 100```

Ou la version Pyspark

```./spark-submit --master spark://0.0.0.0:7077 --name spark-pi  local:///opt/spark/examples/src/main/python/pi.py 10```


Si tout se passe bien, dans l'output il devrait y avoir la ligne suivante

```Pi is roughly 3.1423879142387916```

Pour reference, nous avons envoye un [script](https://github.com/apache/spark/blob/master/examples/src/main/scala/org/apache/spark/examples/SparkPi.scala) calculant une approximation de PI.

Ou la [version PySpark](https://github.com/apache/spark/blob/master/examples/src/main/python/pi.py)

Enfin, on peut démarrer un shell PySpark

```./pyspark```

On peut observer le UI du master au url suivant `http://localhost:9090`.

Enfin on ferme notre cluster

```docker compose down```


## Combiner le cluster avec Jupyter

Blog original [ici](https://nezhar.com/blog/jupyter-notebook-development-workspace-using-docker-and-git/)


Ajouter les lignes suivantes a `docker-compose.yml`:

Par [defaut](https://docs.jupyter.org/en/latest/running.html#starting-the-notebook-server), le port d'un notebook jupyter est 8888, ainsi on devra mapper ce port avec l’extérieur.
Il sera aussi intéressant d'avoir access au SparkUI, lequel est par [défaut au port 4040](https://spark.apache.org/docs/latest/monitoring.html).

```yaml
  jupyter:
    image: jupyter/all-spark-notebook:spark-3.5.0
    volumes:
      - ./work:/home/jovyan/work
      - ./data:/opt/spark-data
    ports:
      - 8888:8888
      - 4040:4040
    container_name: notebook
    command: "start-notebook.sh --NotebookApp.token="
```

Et ajouter `./data:/opt/spark-data` dans les volumes des workers (aussi dans le `docker-compose.yml`).


On relance le cluster avec 
```docker-compose up```

On ouvre retrouve le notebook dans `http://localhost:8888`

On essaie d'abord de rouler un job en mode `local` avec la SparkSession deja utilisée dans les notebooks precedents:

```spark = SparkSession.builder.master("local").getOrCreate()```

Si tout va bien, on essaie en mode cluster.
Pour ce faire, on a besoin du url du master, lequel se trouve dans les logs de docker-compose (ou aussi avec `<master docker ID>:<port du master>`, avec `<port du master>`=7077) 

```spark = SparkSession.builder.master("spark://03e8eecee1b0:7077").getOrCreate()```

Une fois la SparkSession crée, on peut voir le SparkUI au URL suivant:

```http://localhost:4040```