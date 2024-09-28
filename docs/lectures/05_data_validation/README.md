<img style="float: right;" src="../../images/component_engineering.svg" alt="EngineeringAISystems" width="250"/>

## MGL7320 - Ingénierie logicielle des systèmes d'IA
# 05 - Validation et gestion des données

## Prelude

- Quizz - https://ahaslides.com/MSOPC

## Préparation présentation personnelle

- Rencontre Teams

## Validation des données
- :book: [Validation et gestion des données](./05_data_validation.pdf)

## Préparation des données

La préparation des données à fournir aux modèles (ingénierie des caractéristiques / _feature engineering_) est une étape essentielle dans les processus d'apprentissage automatique.

Voici les principales options possibles :
- [Pandas](https://pandas.pydata.org/) pour les jeux de données de taille réduite
- [Spark](https://spark.apache.org/) pour les données massives

### Pandas

Pandas s'exécutant en local, cela ne pose pas de problématique spécifiques d'ingénierie logicielle. Nous ne développerons donc pas la présentation de cette librairie dans ce cours.

Pour la partie pratique, voir le notebook partagé dans le cours [02 - Apprentissage Machine (Machine Learning)](docs/lectures/02_machine_learning).

:bulb: Il est possible de profiter de la puissance de calcul réparti de Spark pour exécuter du "code Pandas" : [Pandas API on Spark](https://spark.apache.org/docs/latest/api/python/user_guide/pandas_on_spark/index.html).

### Spark

#### Théorie
- :book: [INF8200 - Cours 4 - Spark](./05_spark.pdf)

#### Pratique

Tutoriaux à étudier et reproduire en local dans VS Code :
- [Notebook  Google Colab: Introduction à Spark](https://colab.research.google.com/drive/1Enay4TZUyVF7NE1KDUVgyS5oDGI7yMt8?usp=sharing) 
- [Notebook  Google Colab: Combiner des jeux de données](https://colab.research.google.com/drive/1P58LldTNs3XbVvq5Fal-5Asiq1HcghVK?usp=sharing)

## Pour aller plus loin

- Complétez les exercices proposés ici (repris du cours INF8200) : [Spark 2/3 - Cluster et Jupyter](./spark.md)

## Prochaine séance

- Sélection des modèles

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2023-2024
