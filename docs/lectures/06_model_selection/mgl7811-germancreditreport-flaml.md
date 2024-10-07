# Partie 4. Prédiction du risque (Modelling)

Tout d'abord, nous allons diviser le jeu de données en:
- Caractéristiques (X) et variable cible (y)
- Ensemble d'entraînement (75%) et ensemble de test (25%)


```python
# Importation des bibliothèques que nous utiliserons dans cette partie de la classe
from sklearn.model_selection import train_test_split, KFold, cross_val_score # pour séparer les données
from sklearn.metrics import accuracy_score, ConfusionMatrixDisplay, classification_report, f1_score, precision_score, recall_score #Pour évaluer notre modèle

from sklearn.model_selection import GridSearchCV

# Modèles d'algorithmes à comparer
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis
from sklearn.naive_bayes import GaussianNB
from sklearn.svm import SVC
from sklearn.neural_network import MLPClassifier
from xgboost import XGBClassifier
# TODO: Ajouter ici tout nouveau modèle que vous souhaitez essayer (ANN, etc.)

# Création des variables X et y
dataset_ready_x = dataset_ready.drop(['Risk_bad', 'Risk_good', 'Age', 'Sex_male'], axis='columns')
X = dataset_ready_x.values
feature_names = dataset_ready_x.columns

y = dataset_ready['Risk_bad'].values

# Séparation de X et y en version d'entraînement et de test
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.25, random_state=42)
```

## Classifieurs indépendants

Ensuite, nous expérimenterons avec plusieurs modèles pour choisir quelques-uns appropriés. Voici les modèles que nous allons expérimenter:
- [RandomForestClassifier](https://scikit-learn.org/stable/modules/ensemble.html#forests-of-randomized-trees)
- [LogisticRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression)
- [DecisionTreeClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html#sklearn.tree.DecisionTreeClassifier)
- [KNeighborsClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html#sklearn.neighbors.KNeighborsClassifier)
- [GaussianNB](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.GaussianNB.html#sklearn.naive_bayes.GaussianNB)
- [Support Vector Machine Classifier (SVC)](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html#sklearn.svm.SVC)
- [MLPClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html)
- [XGBoost](https://xgboost.readthedocs.io/en/stable/python/python_api.html#module-xgboost.sklearn)
- Plus de classificateurs peuvent être trouvés [ici](https://scikit-learn.org/stable/supervised_learning.html#supervised-learning)



```python
# Expérimentez différents modèles 
classifier = LogisticRegression(solver='liblinear')
# classifier = KNeighborsClassifier()
# classifier = DecisionTreeClassifier()
# classifier = GaussianNB()
# classifier = RandomForestClassifier()
# classifier = SVC()
# classifier = MLPClassifier()
# classifier = XGBClassifier()
# classifier = [...]

classifier_name = classifier.__class__.__name__

scoring_type = 'accuracy'
kfold = KFold(n_splits=5, random_state=42, shuffle=True) # Assurez que toutes les méthodes sont évaluées dans les mêmes données

score = cross_val_score(classifier, X_train, y_train, cv=kfold, scoring=scoring_type)
print(f'Average {scoring_type} performance of the {classifier_name} model = {np.mean(score)}')
```

    Average accuracy performance of the LogisticRegression model = 0.7293333333333333


## AutoML (FLAML)

_A Fast Library for Automated Machine Learning & Tuning_

See https://github.com/microsoft/FLAML
https://microsoft.github.io/FLAML/

!pip install "flaml[automl]"


```python
from flaml import AutoML

automl = AutoML()
automl.fit(X_train, y_train, task="classification", time_budget=180, metric='accuracy')
```

    [flaml.automl.logger: 10-06 16:00:04] {1728} INFO - task = classification
    [flaml.automl.logger: 10-06 16:00:04] {1739} INFO - Evaluation method: cv
    [flaml.automl.logger: 10-06 16:00:04] {1838} INFO - Minimizing error metric: 1-roc_auc
    [flaml.automl.logger: 10-06 16:00:04] {1955} INFO - List of ML learners in AutoML Run: ['lgbm', 'rf', 'xgboost', 'extra_tree', 'xgb_limitdepth', 'sgd', 'lrl1']
    [flaml.automl.logger: 10-06 16:00:04] {2258} INFO - iteration 0, current learner lgbm
    [flaml.automl.logger: 10-06 16:00:04] {2393} INFO - Estimated sufficient time budget=564s. Estimated necessary time budget=13s.
    [flaml.automl.logger: 10-06 16:00:04] {2442} INFO -  at 0.1s,	estimator lgbm's best error=0.2905,	best estimator lgbm's best error=0.2905
    [flaml.automl.logger: 10-06 16:00:04] {2258} INFO - iteration 1, current learner lgbm
    [flaml.automl.logger: 10-06 16:00:04] {2442} INFO -  at 0.1s,	estimator lgbm's best error=0.2835,	best estimator lgbm's best error=0.2835

    .../...
    
    [flaml.automl.logger: 10-06 16:03:02] {2258} INFO - iteration 690, current learner xgb_limitdepth
    [flaml.automl.logger: 10-06 16:03:03] {2442} INFO -  at 178.7s,	estimator xgb_limitdepth's best error=0.2323,	best estimator xgb_limitdepth's best error=0.2323
    [flaml.automl.logger: 10-06 16:03:03] {2258} INFO - iteration 691, current learner xgboost
    [flaml.automl.logger: 10-06 16:03:03] {2442} INFO -  at 179.1s,	estimator xgboost's best error=0.2372,	best estimator xgb_limitdepth's best error=0.2323
    [flaml.automl.logger: 10-06 16:03:03] {2258} INFO - iteration 692, current learner xgb_limitdepth
    [flaml.automl.logger: 10-06 16:03:04] {2442} INFO -  at 179.6s,	estimator xgb_limitdepth's best error=0.2323,	best estimator xgb_limitdepth's best error=0.2323
    [flaml.automl.logger: 10-06 16:03:04] {2258} INFO - iteration 693, current learner xgboost
    [flaml.automl.logger: 10-06 16:03:04] {2442} INFO -  at 179.6s,	estimator xgboost's best error=0.2372,	best estimator xgb_limitdepth's best error=0.2323
    [flaml.automl.logger: 10-06 16:03:04] {2258} INFO - iteration 694, current learner xgboost
    [flaml.automl.logger: 10-06 16:03:04] {2442} INFO -  at 179.7s,	estimator xgboost's best error=0.2372,	best estimator xgb_limitdepth's best error=0.2323
    [flaml.automl.logger: 10-06 16:03:04] {2258} INFO - iteration 695, current learner xgboost
    [flaml.automl.logger: 10-06 16:03:04] {2442} INFO -  at 179.8s,	estimator xgboost's best error=0.2372,	best estimator xgb_limitdepth's best error=0.2323
    [flaml.automl.logger: 10-06 16:03:04] {2258} INFO - iteration 696, current learner xgboost
    [flaml.automl.logger: 10-06 16:03:04] {2442} INFO -  at 180.0s,	estimator xgboost's best error=0.2372,	best estimator xgb_limitdepth's best error=0.2323
    [flaml.automl.logger: 10-06 16:03:04] {2685} INFO - retrain xgb_limitdepth for 0.2s
    [flaml.automl.logger: 10-06 16:03:04] {2688} INFO - retrained model: XGBClassifier(base_score=None, booster=None, callbacks=[],
                  colsample_bylevel=np.float64(0.48908333341027815),
                  colsample_bynode=None,
                  colsample_bytree=np.float64(0.7986228043852982), device=None,
                  early_stopping_rounds=None, enable_categorical=False,
                  eval_metric=None, feature_types=None, gamma=None,
                  grow_policy=None, importance_type=None,
                  interaction_constraints=None,
                  learning_rate=np.float64(0.27323655926425944), max_bin=None,
                  max_cat_threshold=None, max_cat_to_onehot=None,
                  max_delta_step=None, max_depth=7, max_leaves=None,
                  min_child_weight=np.float64(0.3511282865718242), missing=nan,
                  monotone_constraints=None, multi_strategy=None, n_estimators=72,
                  n_jobs=-1, num_parallel_tree=None, random_state=None, ...)
    [flaml.automl.logger: 10-06 16:03:04] {1985} INFO - fit succeeded
    [flaml.automl.logger: 10-06 16:03:04] {1986} INFO - Time taken to find the best model: 165.65140676498413



```python
automl
```




<style>#sk-container-id-3 {
  /* Definition of color scheme common for light and dark mode */
  --sklearn-color-text: black;
  --sklearn-color-line: gray;
  /* Definition of color scheme for unfitted estimators */
  --sklearn-color-unfitted-level-0: #fff5e6;
  --sklearn-color-unfitted-level-1: #f6e4d2;
  --sklearn-color-unfitted-level-2: #ffe0b3;
  --sklearn-color-unfitted-level-3: chocolate;
  /* Definition of color scheme for fitted estimators */
  --sklearn-color-fitted-level-0: #f0f8ff;
  --sklearn-color-fitted-level-1: #d4ebff;
  --sklearn-color-fitted-level-2: #b3dbfd;
  --sklearn-color-fitted-level-3: cornflowerblue;

  /* Specific color for light theme */
  --sklearn-color-text-on-default-background: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, black)));
  --sklearn-color-background: var(--sg-background-color, var(--theme-background, var(--jp-layout-color0, white)));
  --sklearn-color-border-box: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, black)));
  --sklearn-color-icon: #696969;

  @media (prefers-color-scheme: dark) {
    /* Redefinition of color scheme for dark theme */
    --sklearn-color-text-on-default-background: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, white)));
    --sklearn-color-background: var(--sg-background-color, var(--theme-background, var(--jp-layout-color0, #111)));
    --sklearn-color-border-box: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, white)));
    --sklearn-color-icon: #878787;
  }
}

#sk-container-id-3 {
  color: var(--sklearn-color-text);
}

#sk-container-id-3 pre {
  padding: 0;
}

#sk-container-id-3 input.sk-hidden--visually {
  border: 0;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  width: 1px;
}

#sk-container-id-3 div.sk-dashed-wrapped {
  border: 1px dashed var(--sklearn-color-line);
  margin: 0 0.4em 0.5em 0.4em;
  box-sizing: border-box;
  padding-bottom: 0.4em;
  background-color: var(--sklearn-color-background);
}

#sk-container-id-3 div.sk-container {
  /* jupyter's `normalize.less` sets `[hidden] { display: none; }`
     but bootstrap.min.css set `[hidden] { display: none !important; }`
     so we also need the `!important` here to be able to override the
     default hidden behavior on the sphinx rendered scikit-learn.org.
     See: https://github.com/scikit-learn/scikit-learn/issues/21755 */
  display: inline-block !important;
  position: relative;
}

#sk-container-id-3 div.sk-text-repr-fallback {
  display: none;
}

div.sk-parallel-item,
div.sk-serial,
div.sk-item {
  /* draw centered vertical line to link estimators */
  background-image: linear-gradient(var(--sklearn-color-text-on-default-background), var(--sklearn-color-text-on-default-background));
  background-size: 2px 100%;
  background-repeat: no-repeat;
  background-position: center center;
}

/* Parallel-specific style estimator block */

#sk-container-id-3 div.sk-parallel-item::after {
  content: "";
  width: 100%;
  border-bottom: 2px solid var(--sklearn-color-text-on-default-background);
  flex-grow: 1;
}

#sk-container-id-3 div.sk-parallel {
  display: flex;
  align-items: stretch;
  justify-content: center;
  background-color: var(--sklearn-color-background);
  position: relative;
}

#sk-container-id-3 div.sk-parallel-item {
  display: flex;
  flex-direction: column;
}

#sk-container-id-3 div.sk-parallel-item:first-child::after {
  align-self: flex-end;
  width: 50%;
}

#sk-container-id-3 div.sk-parallel-item:last-child::after {
  align-self: flex-start;
  width: 50%;
}

#sk-container-id-3 div.sk-parallel-item:only-child::after {
  width: 0;
}

/* Serial-specific style estimator block */

#sk-container-id-3 div.sk-serial {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: var(--sklearn-color-background);
  padding-right: 1em;
  padding-left: 1em;
}


/* Toggleable style: style used for estimator/Pipeline/ColumnTransformer box that is
clickable and can be expanded/collapsed.
- Pipeline and ColumnTransformer use this feature and define the default style
- Estimators will overwrite some part of the style using the `sk-estimator` class
*/

/* Pipeline and ColumnTransformer style (default) */

#sk-container-id-3 div.sk-toggleable {
  /* Default theme specific background. It is overwritten whether we have a
  specific estimator or a Pipeline/ColumnTransformer */
  background-color: var(--sklearn-color-background);
}

/* Toggleable label */
#sk-container-id-3 label.sk-toggleable__label {
  cursor: pointer;
  display: block;
  width: 100%;
  margin-bottom: 0;
  padding: 0.5em;
  box-sizing: border-box;
  text-align: center;
}

#sk-container-id-3 label.sk-toggleable__label-arrow:before {
  /* Arrow on the left of the label */
  content: "▸";
  float: left;
  margin-right: 0.25em;
  color: var(--sklearn-color-icon);
}

#sk-container-id-3 label.sk-toggleable__label-arrow:hover:before {
  color: var(--sklearn-color-text);
}

/* Toggleable content - dropdown */

#sk-container-id-3 div.sk-toggleable__content {
  max-height: 0;
  max-width: 0;
  overflow: hidden;
  text-align: left;
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-3 div.sk-toggleable__content.fitted {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

#sk-container-id-3 div.sk-toggleable__content pre {
  margin: 0.2em;
  border-radius: 0.25em;
  color: var(--sklearn-color-text);
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-3 div.sk-toggleable__content.fitted pre {
  /* unfitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

#sk-container-id-3 input.sk-toggleable__control:checked~div.sk-toggleable__content {
  /* Expand drop-down */
  max-height: 200px;
  max-width: 100%;
  overflow: auto;
}

#sk-container-id-3 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {
  content: "▾";
}

/* Pipeline/ColumnTransformer-specific style */

#sk-container-id-3 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-3 div.sk-label.fitted input.sk-toggleable__control:checked~label.sk-toggleable__label {
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Estimator-specific style */

/* Colorize estimator box */
#sk-container-id-3 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-3 div.sk-estimator.fitted input.sk-toggleable__control:checked~label.sk-toggleable__label {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-2);
}

#sk-container-id-3 div.sk-label label.sk-toggleable__label,
#sk-container-id-3 div.sk-label label {
  /* The background is the default theme color */
  color: var(--sklearn-color-text-on-default-background);
}

/* On hover, darken the color of the background */
#sk-container-id-3 div.sk-label:hover label.sk-toggleable__label {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-unfitted-level-2);
}

/* Label box, darken color on hover, fitted */
#sk-container-id-3 div.sk-label.fitted:hover label.sk-toggleable__label.fitted {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Estimator label */

#sk-container-id-3 div.sk-label label {
  font-family: monospace;
  font-weight: bold;
  display: inline-block;
  line-height: 1.2em;
}

#sk-container-id-3 div.sk-label-container {
  text-align: center;
}

/* Estimator-specific */
#sk-container-id-3 div.sk-estimator {
  font-family: monospace;
  border: 1px dotted var(--sklearn-color-border-box);
  border-radius: 0.25em;
  box-sizing: border-box;
  margin-bottom: 0.5em;
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-3 div.sk-estimator.fitted {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

/* on hover */
#sk-container-id-3 div.sk-estimator:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-3 div.sk-estimator.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Specification for estimator info (e.g. "i" and "?") */

/* Common style for "i" and "?" */

.sk-estimator-doc-link,
a:link.sk-estimator-doc-link,
a:visited.sk-estimator-doc-link {
  float: right;
  font-size: smaller;
  line-height: 1em;
  font-family: monospace;
  background-color: var(--sklearn-color-background);
  border-radius: 1em;
  height: 1em;
  width: 1em;
  text-decoration: none !important;
  margin-left: 1ex;
  /* unfitted */
  border: var(--sklearn-color-unfitted-level-1) 1pt solid;
  color: var(--sklearn-color-unfitted-level-1);
}

.sk-estimator-doc-link.fitted,
a:link.sk-estimator-doc-link.fitted,
a:visited.sk-estimator-doc-link.fitted {
  /* fitted */
  border: var(--sklearn-color-fitted-level-1) 1pt solid;
  color: var(--sklearn-color-fitted-level-1);
}

/* On hover */
div.sk-estimator:hover .sk-estimator-doc-link:hover,
.sk-estimator-doc-link:hover,
div.sk-label-container:hover .sk-estimator-doc-link:hover,
.sk-estimator-doc-link:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

div.sk-estimator.fitted:hover .sk-estimator-doc-link.fitted:hover,
.sk-estimator-doc-link.fitted:hover,
div.sk-label-container:hover .sk-estimator-doc-link.fitted:hover,
.sk-estimator-doc-link.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

/* Span, style for the box shown on hovering the info icon */
.sk-estimator-doc-link span {
  display: none;
  z-index: 9999;
  position: relative;
  font-weight: normal;
  right: .2ex;
  padding: .5ex;
  margin: .5ex;
  width: min-content;
  min-width: 20ex;
  max-width: 50ex;
  color: var(--sklearn-color-text);
  box-shadow: 2pt 2pt 4pt #999;
  /* unfitted */
  background: var(--sklearn-color-unfitted-level-0);
  border: .5pt solid var(--sklearn-color-unfitted-level-3);
}

.sk-estimator-doc-link.fitted span {
  /* fitted */
  background: var(--sklearn-color-fitted-level-0);
  border: var(--sklearn-color-fitted-level-3);
}

.sk-estimator-doc-link:hover span {
  display: block;
}

/* "?"-specific style due to the `<a>` HTML tag */

#sk-container-id-3 a.estimator_doc_link {
  float: right;
  font-size: 1rem;
  line-height: 1em;
  font-family: monospace;
  background-color: var(--sklearn-color-background);
  border-radius: 1rem;
  height: 1rem;
  width: 1rem;
  text-decoration: none;
  /* unfitted */
  color: var(--sklearn-color-unfitted-level-1);
  border: var(--sklearn-color-unfitted-level-1) 1pt solid;
}

#sk-container-id-3 a.estimator_doc_link.fitted {
  /* fitted */
  border: var(--sklearn-color-fitted-level-1) 1pt solid;
  color: var(--sklearn-color-fitted-level-1);
}

/* On hover */
#sk-container-id-3 a.estimator_doc_link:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

#sk-container-id-3 a.estimator_doc_link.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-3);
}
</style><div id="sk-container-id-3" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>AutoML(append_log=False, auto_augment=True, custom_hp={},
       cv_score_agg_func=None, early_stop=False, ensemble=False,
       estimator_list=&#x27;auto&#x27;, eval_method=&#x27;auto&#x27;, fit_kwargs_by_estimator={},
       force_cancel=False, free_mem_ratio=0, hpo_method=&#x27;auto&#x27;,
       keep_search_state=False, learner_selector=&#x27;sample&#x27;, log_file_name=&#x27;&#x27;,
       log_training_metric=False, log_type=&#x27;better&#x27;, max_iter=None,
       mem_thres=4294967296, metric=&#x27;auto&#x27;, metric_constraints=[],
       min_sample_size=10000, mlflow_exp_name=None, mlflow_logging=True,
       model_history=False, n_concurrent_trials=1, n_jobs=-1, n_splits=5,
       pred_time_limit=inf, preserve_checkpoint=True, ...)</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item"><div class="sk-estimator fitted sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-3" type="checkbox" checked><label for="sk-estimator-id-3" class="sk-toggleable__label fitted sk-toggleable__label-arrow fitted">&nbsp;AutoML<span class="sk-estimator-doc-link fitted">i<span>Fitted</span></span></label><div class="sk-toggleable__content fitted"><pre>AutoML(append_log=False, auto_augment=True, custom_hp={},
       cv_score_agg_func=None, early_stop=False, ensemble=False,
       estimator_list=&#x27;auto&#x27;, eval_method=&#x27;auto&#x27;, fit_kwargs_by_estimator={},
       force_cancel=False, free_mem_ratio=0, hpo_method=&#x27;auto&#x27;,
       keep_search_state=False, learner_selector=&#x27;sample&#x27;, log_file_name=&#x27;&#x27;,
       log_training_metric=False, log_type=&#x27;better&#x27;, max_iter=None,
       mem_thres=4294967296, metric=&#x27;auto&#x27;, metric_constraints=[],
       min_sample_size=10000, mlflow_exp_name=None, mlflow_logging=True,
       model_history=False, n_concurrent_trials=1, n_jobs=-1, n_splits=5,
       pred_time_limit=inf, preserve_checkpoint=True, ...)</pre></div> </div></div></div></div>




```python
# Print the best model and its parameters
print(automl.best_estimator)
print(automl.best_config)
```

    xgb_limitdepth
    {'n_estimators': 72, 'max_depth': 7, 'min_child_weight': np.float64(0.3511282865718242), 'learning_rate': np.float64(0.27323655926425944), 'subsample': np.float64(0.7245303466863017), 'colsample_bylevel': np.float64(0.48908333341027815), 'colsample_bytree': np.float64(0.7986228043852982), 'reg_alpha': np.float64(3.001024163238421), 'reg_lambda': np.float64(18.282295462121635)}



```python
# Prédire les données de test
y_pred = automl.predict(X_test)
print(f"Accuracy of {automl.best_estimator}'s prediction is {accuracy_score(y_test,y_pred)}")
```

    Accuracy of xgb_limitdepth's prediction is 0.756
