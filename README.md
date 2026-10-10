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
