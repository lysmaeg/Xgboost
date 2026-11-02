# TP XGBoost à trou — Corrigé

## Réponses aux trous (valeurs à tester)

On lance avec la 1re valeur, puis la 2e, et on compare.

| Section | Trou | Valeurs à tester | Ce qu'on observe |
| --- | --- | --- | --- |
| 3. `eta` | `evaluer(eta = ____)` | `0.01` puis `0.3` | 0.01 (+ 100 arbres) → AUC plus basse (sous-apprentissage) ; 0.3 → apprend plus vite, AUC plus haute |
| 4. `max_depth` | `evaluer(max_depth = ____)` | `1` puis `12` | depth 1 → simple, AUC modérée ; depth 12 → AUC train très haute, AUC test qui décroche → surapprentissage |
| 5. `nrounds` | `evaluer(nrounds = ____)` | `10` puis `500` | 10 → sous-apprentissage ; 500 → train ↑↑, test plafonne (rendement décroissant) |

*La section 6 (validation croisée) n'a pas de trou : on exécute et on lit `best_iteration`.*

## Réponses aux questions d'interprétation

**Section 3 — `eta` (taux d'apprentissage).**
`eta` fixe le poids de chaque nouvel arbre. Petit (0.01) → chaque arbre contribue peu, l'apprentissage est lent : avec seulement 100 arbres, le modèle n'a pas eu le temps d'apprendre → AUC plus basse (sous-apprentissage). Plus grand (0.3) → apprend plus vite, meilleure AUC ici. (Un `eta` trop grand peut aussi rendre l'apprentissage instable ; on compense un petit `eta` par plus d'arbres.)

**Section 4 — `max_depth` (profondeur).**
Non, des arbres plus profonds ne généralisent pas forcément mieux. Avec `max_depth = 12`, l'AUC train devient très élevée mais l'AUC test est nettement en dessous : **l'écart train/test se creuse**. C'est du **surapprentissage (overfitting)** : le modèle mémorise le bruit des données d'entraînement au lieu d'apprendre des règles générales. Des arbres trop profonds sont le principal levier de surapprentissage dans XGBoost.

**Section 5 — `nrounds` (nombre d'arbres).**
De 10 à 500 : l'AUC train continue de monter (le modèle colle de mieux en mieux au train), mais l'AUC **test plafonne** et finit par stagner, voire baisser. Le gain n'est pas infini : **rendement décroissant**. Ajouter des arbres au-delà d'un certain point n'améliore plus la généralisation et coûte du temps de calcul.

**Section 6 — validation croisée + arrêt anticipé.**
Fixer `nrounds` à la main, c'est deviner. La validation croisée teste la performance sur des données non vues et **`early_stopping_rounds`** arrête d'ajouter des arbres dès que l'AUC de validation ne progresse plus pendant N tours. On obtient ainsi automatiquement un bon nombre d'arbres (`best_iteration`), sans tâtonner ni risquer de surapprendre en mettant trop d'arbres.

