<img style="float: right;" src="../../images/component_engineering.svg" alt="EngineeringAISystems" width="250"/>

## MGL7320 - Ingénierie logicielle des systèmes d'IA
# 07 - Intelligence Artificielle Générative

## Prelude

- Quizz - [https://ahaslides.com/](https://ahaslides.com/)

## Lecture du jour

- :book: [IA Générative](./07_gen_ai.pdf)

## :pencil: Mise en pratique des GML (_LLM_)

### Construire une application à base de GML

Le but de cet exercice est d'écrire et d'exécuter le code suivant (ou équivalent), lequel fait appel à un GML (_LLM_).

```python
from langchain_core.messages import HumanMessage, SystemMessage

messages = [
    SystemMessage(content="Translate the following from English into French"),
    HumanMessage(content="I love programming."),
]

model.invoke(messages)
```

- Pour cela, 
    - soit utiliser à l'API [OpenAI](https://python.langchain.com/docs/tutorials/llm_chain/) ou équivalents (options _a priori_ payantes), 
    - soit en local avec [ChatOllama](https://python.langchain.com/docs/integrations/chat/ollama/) ou [LM Studio](https://lmstudio.ai) - options gratuites mais demandant un ordinateur suffisamment puissant - ;
- Pour l'option locale, choisissez au départ un [modèle](https://ollama.com/library) suffisamment petit, tel que `mixtral:8x7b`. Celui-ci nécessitera bien moins de ressources, tout en étant téléchargé bien plus rapidement ; 
- LangSmith est optionnel. :bulb: Il peut être notamment remplacé par [Phoenix](https://phoenix.arize.com) ;
- Si vous n'avez pas la possibilité d'exécuter par vous-même ce code (ordinateur pas assez puissant, etc.), regroupez-vous avec un autre étudiant qui en est capable.

### Utiliser LangChain pour analyser des images

Le but de l'exercice est d'utiliser un GML pour décrire l'image suivante : 
[https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg](https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg)

#### Références 
- [How to use multimodal prompts](https://python.langchain.com/docs/how_to/multimodal_prompts/)
- [Multi-modal](https://python.langchain.com/docs/integrations/chat/ollama/#multi-modal) ChatOllama

### Développer un RAG à base de LangChain

Le but de l'exercice est de pouvoir interroger les informations fournioes par le blog [LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/), en particulier afin de pouvoir répondre à la question suivante : "What is Task Decomposition?".

#### Références
- [Build a Retrieval Augmented Generation (RAG) App](https://python.langchain.com/docs/tutorials/rag/)
- [Chroma DB](https://github.com/chroma-core/chroma)
- [OllamaEmbeddings]https://python.langchain.com/docs/integrations/text_embedding/ollama/

:bulb: La phase d'encodage du texte de référence peut prendre plusieurs minutes.

## Projet en équipe

Compléter la [mise à niveau Smart-Meter](../projet_equipe.md) 

Calendrier
- Présentation (non évaluée) du plan du projet en équipe pour le 12 novembre
- Remise et présentations (incluant démos) par les étudiants de leur projets en équipe pour le 10 décembre.

## Travail personnel pour les prochaines semaines

- [ ] Si possible, compléter les exercices du jour
- [ ] **Constituez les équipes pour le projet en commun (3 à 4 membres)**
- [ ] Récupérer _et parcourir_ le livre [The Big Book of Generative AI](https://www.databricks.com/resources/ebook/big-book-generative-ai)  (copie gratuite)
- [ ] Récupérer _et parcourir_ le livre [Augment your LLMs using RAG](https://www.databricks.com/resources/ebook/train-llms-your-data) (copie gratuite)
- [ ] Lire et visionner [IA génératives et méthodes de diffusion](https://scienceetonnante.com/2023/01/13/stable-diffusion/)
- [ ] Visionner [Comment fonctionne ChatGPT ?](https://scienceetonnante.com/2023/04/14/comment-fonctionne-chatgpt/)

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2023-2024


