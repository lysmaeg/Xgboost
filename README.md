Présentation XGBoost - M2 MAS 2026 / 2027

Canva de la présentation : [Lien](https://canva.link/clcmkx0zxjnojx2)


## Comparaison : Gradient Boosting classique vs XGBoost

Cette partie compare le **Gradient Boosting classique** et **XGBoost**, deux méthodes d'ensemble à base d'arbres de décision construits de manière séquentielle.

**Idée générale** : imagine une équipe d'experts qui passent l'un après l'autre sur un même problème. Chaque nouvel expert (un arbre) corrige les erreurs laissées par les précédents. Le Gradient Boosting et XGBoost fonctionnent ainsi ; XGBoost est une version **plus rigoureuse, plus contrôlée et plus rapide**.

### Tableau comparatif

| Critère | Gradient Boosting classique | XGBoost | Ce que ça change en pratique |
|---|---|---|---|
| **Principe** | Ajoute des arbres un par un ; chaque arbre corrige les erreurs du modèle courant. | Même principe : des arbres ajoutés successivement pour améliorer les prédictions. | Les deux sont des modèles « en équipe » : la prédiction finale est la somme de tous les arbres. |
| **Construction des arbres** | Utilise le **gradient** de la perte. | Utilise le gradient **et la dérivée seconde (hessienne)**. | XGBoost choisit des corrections plus précises grâce à l'information de courbure. |
| **Régularisation** | Indirecte : profondeur des arbres, taux d'apprentissage. | **Explicite** : `gamma`, `lambda`, `alpha`. | Plus de réglages pour éviter que le modèle apprenne le bruit. |
| **Fonction de perte** | Choisie selon le problème : erreur quadratique, perte logistique, etc. | Même principe : `reg:squarederror` (régression), `binary:logistic` (classification binaire), `multi:softprob` (classification multiclasse). | Les deux acceptent plusieurs pertes ; XGBoost demande seulement qu'elles soient dérivables deux fois (gradient et hessienne). |
| **Hyperparamètres** | Principalement `n_estimators`, `learning_rate` et `max_depth`. | Les mêmes, plus des paramètres de régularisation : `gamma`, `lambda`, `alpha`. | Plus flexible pour contrôler le surapprentissage, mais plus complexe à régler. |

### Synthèse

Le Gradient Boosting et XGBoost reposent sur le **même principe** : construire des arbres les uns après les autres, chacun corrigeant les erreurs de l'ensemble précédent. Les deux acceptent différentes fonctions de perte et partagent les mêmes hyperparamètres de base (`n_estimators`, `learning_rate`, `max_depth`).

La différence se situe dans **la façon de construire les arbres** :

- **Plus d'information** : XGBoost exploite le gradient **et** la dérivée seconde (hessienne) de la perte, ce qui lui permet des corrections plus précises que le gradient seul.
- **Complexité contrôlée** : XGBoost intègre une régularisation explicite (`gamma`, `lambda`, `alpha`) dans la fonction qu'il minimise, alors que le Gradient Boosting classique se limite à des contraintes indirectes (profondeur, taux d'apprentissage).
- **Contrepartie** : ces paramètres supplémentaires rendent XGBoost plus flexible pour limiter le surapprentissage, mais aussi plus complexe à calibrer.

**En résumé** : XGBoost peut être vu comme une version « régularisée et optimisée » du Gradient Boosting. Il offre plus de leviers pour maîtriser le modèle, au prix d'un réglage plus délicat.

### Exercice d'application

**Contexte** : une enseigne veut prédire si un client rachètera un produit le mois prochain (oui / non). Le jeu de données contient 100 000 lignes et plusieurs variables avec des **valeurs manquantes**. Un premier modèle XGBoost obtient une AUC de **0,95 sur les données d'entraînement** mais seulement de **0,80 sur les données de test**.

**Questions**

1. Quel `objective` faut-il choisir pour ce problème ?
2. Que révèle l'écart entre les scores d'entraînement et de test ?
3. Citez deux hyperparamètres **communs** au Gradient Boosting et à XGBoost, et deux hyperparamètres **propres à XGBoost**, que l'on peut modifier pour corriger ce problème. Dans quel sens faut-il les modifier ?
4. Les données contiennent des valeurs manquantes. Faut-il les supprimer ou les remplacer avant l'entraînement ? Justifiez.
5. On diminue `learning_rate` de 0,3 à 0,05. Quel autre hyperparamètre faut-il ajuster, et dans quel sens ?
6. Complétez le code suivant :

```python
import xgboost as xgb

model = xgb.XGBClassifier(
    objective="...",
    n_estimators=...,
    learning_rate=...,
    max_depth=...,
    gamma=...,
    reg_lambda=...,
    reg_alpha=...,
)
model.fit(X_train, y_train)
```

<details>
<summary><b>Corrigé</b></summary>

1. **`binary:logistic`** : c'est une classification binaire (le client rachète ou non), et le modèle renvoie une probabilité.
2. Il révèle un **surapprentissage** : le modèle a trop bien appris les données d'entraînement (y compris le bruit) et généralise mal.
3. Pour réduire le surapprentissage :
   - **Communs** : diminuer `max_depth` (arbres moins profonds) et diminuer `learning_rate` (corrections plus prudentes, avec davantage d'arbres).
   - **Propres à XGBoost** : augmenter `gamma` (on ne coupe que si le gain est suffisant), augmenter `lambda` (corrections plus douces) et/ou `alpha` (certains poids tombent à zéro).
4. **Ni l'un ni l'autre, ce n'est pas obligatoire.** XGBoost apprend automatiquement, à chaque division, une **direction par défaut** pour les valeurs manquantes. On peut donc les conserver telles quelles, ce qui évite une imputation parfois arbitraire.
5. Il faut **augmenter `n_estimators`** : avec un pas plus petit, chaque arbre corrige moins, donc il en faut davantage pour atteindre la même qualité. L'entraînement est plus lent, mais plus stable.
6. Exemple de configuration cohérente :

```python
model = xgb.XGBClassifier(
    objective="binary:logistic",
    n_estimators=500,
    learning_rate=0.05,
    max_depth=4,
    gamma=1,
    reg_lambda=5,
    reg_alpha=0.5,
)
```

Les valeurs exactes dépendent des données : on les choisit en pratique par validation croisée. L'important est le **sens** de chaque réglage (arbres peu profonds, régularisation renforcée, `learning_rate` faible avec beaucoup d'arbres).

</details>
