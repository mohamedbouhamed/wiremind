
# Orthogonal Statistical Learning — Résumé synthétique

Ce dépôt résume l'article **"Orthogonal Statistical Learning"** (Foster & Syrgkanis), qui traite de l'estimation robuste de paramètres causaux en présence de **paramètres de nuisance** inconnus.

---

## Objectif

Estimer un **paramètre cible** $\theta$ (ex. : effet de traitement, politique optimale) à partir d'un risque de population :

```math
L_D(\theta, g)
```

où $g_0$ est un **paramètre de nuisance** inconnu (score de propension $e_0(W)$, fonction de régression $f_0(W)$...).

**Défi :** Minimiser l'**excès de risque** suivant, même lorsque $g_0$ doit être estimé à partir des données :

```math
L_D(\hat{\theta}, g_0) - L_D(\theta^*, g_0)
```

---

## Défis principaux

- **Propagation d'erreur** : les erreurs d'estimation sur $g$ contaminent fortement l'estimation de $\theta$  
- **Modèles complexes** : les garanties classiques échouent avec des estimateurs non-paramétriques  
- **Échantillons finis** : les garanties asymptotiques sont insuffisantes en pratique

---

## Contributions clés

### 1. Orthogonalité de Neyman

Condition cruciale :

```math
D_g D_\theta L_D(\theta, g)[\theta' - \theta, g' - g] = 0
```

Cela découple l'erreur sur $\hat{g}$ de celle sur $\hat{\theta}$.

**Effet :** L’erreur sur $\hat{g}$ a un **impact d’ordre supérieur**, typiquement :

```math
\|\hat{g} - g_0\|^4 \quad \text{au lieu de} \quad \|\hat{g} - g_0\|
```

---

### 2. Méta-algorithme en deux étapes

1. Estimer $\hat{g}$ sur un sous-échantillon $S_1$  
2. Estimer $\hat{\theta}$ sur $S_2$ avec une perte orthogonale

Cela évite le surajustement et permet une analyse indépendante des deux étapes.

---

### 3. Taux oracle

Si $\|\hat{g} - g_0\| = o(n^{-1/4})$, alors :

```math
L_D(\hat{\theta}, g_0) - L_D(\theta^*, g_0) = O(1/n)
```

On obtient les **mêmes performances** que si $g_0$ était connu.

---

### 4. Deux régimes de performance
Ce taux dépend aussi de la structure géométrique de la fonction de perte :

- **Fast rates** : si la perte corrigée est fortement convexe (ex. ridge), alors l’erreur sur \(\hat{\theta}\) décroît en \(1/n\), à condition que \(\|\hat{g} - g_0\| = o(n^{-1/4})\)
- **Slow rates** : si la perte n’est pas fortement convexe, on a un taux plus lent (\(1/\sqrt{n}\)), en demandant \(\|\hat{g} - g_0\| = o(n^{-1/2})\)

Le choix de la classe $\Theta$ influe sur la rapidité de convergence.


---

## Exemples d’application

### Estimation d'effets de traitement hétérogènes (CATE)

Modèle utilisé :

```math
Y = T \cdot \theta_0(X) + f_0(W) + \varepsilon_1, \quad \mathbb{E}[\varepsilon_1 \mid X, W, T] = 0  
T = e_0(W) + \varepsilon_2, \quad \mathbb{E}[\varepsilon_2 \mid X, W] = 0
```

- **Perte R-Loss** :

```math
\ell(\theta, g) = (Y - f_0(W) - \theta(X)(T - e_0(W)))^2
```

- **Résultat** : taux optimal si $\|\hat{g} - g_0\| = o(n^{-1/4})$

---
## Avantages

- **Flexible** : le cadre est **agnostique** aux choix d'algorithmes — on peut utiliser des réseaux de neurones, forêts aléatoires, boosting, etc., aussi bien pour les fonctions de nuisance $\hat{g}$ que pour le paramètre cible $\hat{\theta}$
- **Robuste** : même si $\theta$ est mal spécifié ou que $\hat{g}$ est imparfait, la structure orthogonale **amortit l'erreur**
- **Efficace** : si $\hat{g}$ est estimé avec assez de précision, $\hat{\theta}$ atteint un **taux oracle**, comme si les nuisances étaient connues
- **Compatible ML** : permet d'exploiter la puissance du machine learning (ex. : RNNs, XGBoost, transformers) sans compromettre la rigueur statistique
- **Domaines d'application variés** : santé personnalisée, ciblage de politiques publiques, gestion des données manquantes, adaptation de domaine...
