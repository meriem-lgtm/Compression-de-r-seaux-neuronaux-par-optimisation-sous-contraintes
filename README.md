# Compression de Réseaux Neuronaux par Optimisation sous Contraintes

## Présentation

Ce projet étudie différentes techniques de compression de réseaux neuronaux afin de réduire leur taille mémoire et leur coût d'inférence tout en conservant des performances élevées.

L'objectif est de comparer plusieurs approches d'optimisation et de compression appliquées au réseau LeNet sur le dataset MNIST.

Les expériences ont été réalisées avec PyTorch et évaluées à l'aide de métriques réelles : précision, temps d'inférence, taille disque, consommation mémoire RAM et robustesse au bruit.

---

## Objectifs du projet

* Entraîner un modèle de référence LeNet sur MNIST.
* Comparer plusieurs optimiseurs.
* Étudier l'effet des régularisations L1 et L2.
* Réaliser un pruning non structuré.
* Réaliser un pruning structuré.
* Appliquer une quantification dynamique int8.
* Mesurer les gains de compression obtenus.
* Évaluer l'impact sur la précision et la robustesse.

---

## Technologies utilisées

* Python 3
* PyTorch
* Torchvision
* NumPy
* Pandas
* Matplotlib
* psutil
* Google Colab / Jupyter Notebook

---

## Architecture du modèle

Le projet utilise une architecture LeNet-5 adaptée au dataset MNIST.

### Structure

* Conv2D (1 → 6)
* ReLU
* Average Pooling
* Conv2D (6 → 16)
* ReLU
* Average Pooling
* Fully Connected (400 → 120)
* Fully Connected (120 → 84)
* Fully Connected (84 → 10)

Nombre total de paramètres :

61 706 paramètres

---

## Dataset

### MNIST

Le dataset MNIST contient des images manuscrites de chiffres de 0 à 9.

Pour accélérer les expérimentations :

* 6000 images pour l'entraînement
* 1500 images pour les tests

Prétraitement :

* Conversion en tenseurs
* Normalisation

---

## Optimiseurs étudiés

### Adam

* Convergence rapide
* Très stable
* Meilleur compromis global

### SGD

* Bonne précision finale
* Plus sensible au learning rate

### L-BFGS

* Très coûteux en temps
* Instable sur mini-batches
* Non recommandé pour ce problème

---

## Régularisation

### L1

Favorise la sparsité des poids.

Résultat :

* Forte concentration des poids autour de zéro
* Légère perte de précision

### L2

Réduit la magnitude des poids.

Résultat :

* Bonne généralisation
* Perte de précision limitée

---

## Pruning Non Structuré

Méthode :

Magnitude-Based Pruning avec :

torch.nn.utils.prune.l1_unstructured

Taux étudiés :

* 10 %
* 20 %
* 30 %
* 40 %
* 50 %
* 60 %
* 70 %
* 80 %
* 90 %

Résultat :

Le seuil optimal observé est proche de 40 % de pruning avec une perte de précision inférieure à 2 %.

---

## Pruning Structuré

Méthode :

torch.nn.utils.prune.ln_structured

Suppression complète de filtres convolutionnels.

Résultat :

* Compression réelle du réseau
* Dégradation plus importante de la précision

---

## Quantification Dynamique

Méthode :

Quantification dynamique int8.

Résultats observés :

* Réduction de taille ≈ 70 %
* Compression ≈ 3.3×
* Impact minimal sur la précision

---

## Métriques évaluées

### Précision (Accuracy)

Mesure des performances de classification.

### Sparsité

Pourcentage réel de poids nuls.

### Taille disque

Mesurée directement à partir du modèle sauvegardé.

### Consommation RAM

Mesurée avec psutil.

### Temps d'inférence

Mesuré avec time.perf_counter().

### Robustesse

Évaluation sur données bruitées.

---

## Résultats principaux

| Technique           | Accuracy |
| ------------------- | -------- |
| Référence Adam      | 96.33 %  |
| SGD                 | 96.40 %  |
| L-BFGS              | 10.07 %  |
| L1                  | 94.40 %  |
| L2                  | 95.67 %  |
| Pruning 40 %        | 94.67 %  |
| Quantification int8 | 96.20 %  |

---

## Conclusion

Les résultats montrent que la quantification dynamique constitue la meilleure solution pour un déploiement sur systèmes embarqués ou Edge AI.

Elle permet :

* une réduction importante de la taille du modèle ;
* une conservation quasi totale des performances ;
* une faible dégradation du temps d'inférence.

Le pruning non structuré offre également une compression intéressante jusqu'à environ 40 % de sparsité avant une chute significative de la précision.

---

## Recommandation Finale

Stratégie recommandée :

1. Entraînement avec Adam.
2. Régularisation L1.
3. Pruning non structuré à 40 %.
4. Quantification dynamique int8.

Cette combinaison fournit le meilleur compromis entre :

* précision,
* taille mémoire,
* coût d'inférence,
* déploiement sur appareils contraints.

---

## Auteur

Projet réalisé dans le cadre du module :

**Compression de réseaux neuronaux par optimisation sous contraintes**

Année universitaire 2025-2026.
