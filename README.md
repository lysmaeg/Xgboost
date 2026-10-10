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

### Exercice d'application : QCM

Une seule réponse est correcte par question.

**1. Quelle information XGBoost utilise-t-il en plus du gradient de la fonction de perte pour construire ses arbres ?**
- A. La moyenne des erreurs
- B. La dérivée seconde (hessienne)
- C. La médiane des prédictions
- D. Le nombre de variables

**2. Quel est le principe commun au Gradient Boosting classique et à XGBoost ?**
- A. Entraîner des arbres indépendants en parallèle et faire une moyenne
- B. Utiliser un seul arbre très profond
- C. Ajouter des arbres successifs, chacun corrigeant les erreurs du modèle courant
- D. Réduire le nombre de variables avant l'entraînement

**3. Quel paramètre de XGBoost représente le coût d'ajout d'une feuille supplémentaire ?**
- A. `gamma`
- B. `lambda`
- C. `alpha`
- D. `n_estimators`

**4. Parmi ces propositions, laquelle est vraie pour XGBoost par rapport au Gradient Boosting classique ?**
- A. Il ne peut jamais surapprendre
- B. Il offre plus de leviers pour contrôler le surapprentissage, mais est plus complexe à régler
- C. Il n'a aucun hyperparamètre à régler
- D. Il est toujours plus performant

**5. Quels sont les hyperparamètres communs aux deux algorithmes, et ceux propres à XGBoost ?**
- A. Communs : `gamma`, `lambda`, `alpha` ; propres à XGBoost : `n_estimators`, `learning_rate`
- B. Communs : `gamma` et `lambda` ; propres à XGBoost : `max_depth`
- C. Communs : `n_estimators`, `learning_rate`, `max_depth` ; propres à XGBoost : `gamma`, `lambda`, `alpha`
- D. Aucun paramètre n'est commun aux deux algorithmes

<details>
<summary><b>Corrigé</b></summary>

1. **B.** XGBoost s'appuie sur une approximation d'ordre 2 de la perte : il utilise le gradient et la hessienne, ce qui permet des corrections plus précises que le gradient seul.
2. **C.** Les deux sont des méthodes de *boosting* : les arbres sont construits un par un et la prédiction finale est la somme de leurs contributions. La proposition A décrit plutôt une forêt aléatoire.
3. **A.** `gamma` pénalise le nombre de feuilles (terme $\gamma T$) : une coupure n'est effectuée que si le gain dépasse ce coût. `lambda` et `alpha` pénalisent les valeurs des feuilles (L2 et L1).
4. **B.** XGBoost ajoute des paramètres de régularisation, ce qui donne plus de contrôle mais demande un réglage plus fin. Il peut surapprendre et n'est pas automatiquement meilleur sur tous les jeux de données.
5. **C.** `n_estimators` (nombre d'arbres), `learning_rate` (contribution de chaque arbre) et `max_depth` (profondeur des arbres) sont communs. `gamma`, `lambda` et `alpha` sont les paramètres de régularisation propres à XGBoost dans cette comparaison.

</details>
