
# Corrigé du notebook `TP_XGBoost_a_trou.ipynb`.

## Réponses aux trous (code)

| Section | Trou | Réponse |
| --- | --- | --- |
| 2. Split train / test | `p = ____` | `0.70` |
| 3. Validation croisée | `nfold = ____` | `10` |
| 4. Modèle final | `nrounds = ____` | `ite_opt` |

### À expliquer — pourquoi `p = 0.70` ?

C'est une convention, pas une règle absolue. En séparant les données, on arbitre entre deux besoins, un jeu d'entraînement assez grand pour bien apprendre, et un jeu de test assez grand pour une évaluation fiable. 70/30 (ou 80/20) est le compromis le plus courant. Ici, 50 000 vols, 35 000 pour l'entraînement, 15 000 pour le test (largement suffisant, l'AUC est stable).

### À expliquer — pourquoi `nfold = 10` ?

La validation croisée découpe le train en 10 groupes, on entraîne sur 9, on valide sur 1, et on tourne. Chaque groupe sert une fois de validation. On moyenne les 10 résultats pour obtenir une estimation plus stable de la performance.

Pourquoi 10 ? C'est un compromis, comme pour le split. 
1. Plus de folds = estimation plus précise, mais plus lent. À l'extrême, autant de folds que d'observations très précis, mais des milliers d'entraînements, trop lent. Moins de folds = plus rapide, mais estimation plus instable.
2. 10 (parfois 5) est le choix standard, reconnu comme le bon équilibre entre fiabilité et temps de calcul. On le retrouve partout dans les cours et les articles.

10 n'est pas une valeur optimale calculée, c'est le palier standard, assez précis sans être trop lent. 5 folds donnerait un résultat similaire, un peu moins stable.

### À expliquer — pourquoi `nrounds = ite_opt` ?

`ite_opt` est le nombre d'arbres choisi par la validation croisée (`best_iteration`). On le **réutilise** pour le modèle final au lieu de refixer un nombre à la main : c'est tout l'intérêt d'avoir fait la CV à l'étape précédente.

## Réponses aux questions d'interprétation

**Question 1 — Classes équilibrées ?**

Non, environ 24 % de retards contre environ 76 % à l'heure.

Avec des classes déséquilibrées, l'accuracy devient trompeuse. Un modèle qui prédirait "jamais en retard" pour tout le monde aurait déjà ~76 % d'accuracy sans rien apprendre, alors qu'il ne détecte aucun retard, donc il est inutile.
Conclusion, c'est pour ça qu'on suit l'AUC et pas l'accuracy. L'AUC mesure la capacité à séparer les deux classes, sans se laisser berner par la classe majoritaire. Donc, devant une accuracy de 90 %, toujours se demander d'abord si les classes sont équilibrées.

**Question 2 — Même proportion dans train et test, pourquoi c'est bien ?**

Parce que le tirage est stratifié, train et test gardent la même proportion de retards ( environ 24 %). On évalue ainsi le modèle sur un échantillon représentatif de l'ensemble. Un tirage purement aléatoire pourrait, par malchance, sur- ou sous-représenter les retards dans le test et fausser la mesure de performance.

