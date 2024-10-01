<img style="float: right;" src="../../images/component_engineering.svg" alt="EngineeringAISystems" width="250"/>

## MGL7320 - Ingénierie logicielle des systèmes d'IA
# 05 - Ingénierie des caractéristiques

## Prelude

- Quizz architecture - [https://ahaslides.com/MSOPC](https://ahaslides.com/MSOPC)

## Préparation présentation personnelle du 15 octobre

- [Rencontre Teams](https://teams.microsoft.com/l/meetup-join/19%3ameeting_NzkzYjY2NTItYWMyNS00NGZmLWE3NDUtMmMyYmE1NzVhNWNm%40thread.v2/0?context=%7b%22Tid%22%3a%2212cb4e1a-42da-491c-90e1-7a7a9753506f%22%2c%22Oid%22%3a%22de64bd01-d8ce-40e7-8f6c-2b5de9f33661%22%7d)

## Validation des données
- :book: [Validation et gestion des données](./05_data_validation.pdf)

## Préparation des données

La préparation des données à fournir aux modèles (ingénierie des caractéristiques / _feature engineering_) est une étape essentielle dans les processus d'apprentissage automatique.

Voici les principales options de transformation possibles :
- [Pandas](https://pandas.pydata.org/) pour les jeux de données de taille réduite
- [Spark](https://spark.apache.org/) pour les données massives

### Pandas

_Pandas s'exécutant en local, cela ne pose pas de problématique spécifiques d'ingénierie logicielle. Nous ne développerons donc pas la présentation de cette librairie dans ce cours._

Pour la partie pratique, voir le notebook partagé dans le cours [02 - Apprentissage Machine (Machine Learning)](docs/lectures/02_machine_learning).

:bulb: Il est possible de profiter de la puissance de calcul réparti de Spark pour y exécuter du "code Pandas" : [Pandas API on Spark](https://spark.apache.org/docs/latest/api/python/user_guide/pandas_on_spark/index.html).

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
