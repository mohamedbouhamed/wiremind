# Orthogonal Statistical Learning — Résumé synthétique

Ce dépôt résume l'article **"Orthogonal Statistical Learning"** (Foster & Syrgkanis), qui traite de l'estimation robuste de paramètres causaux en présence de **paramètres de nuisance** inconnus.

---

## Objectif

Estimer un **paramètre cible** $\theta$ (ex. : effet de traitement, politique optimale) à partir d'un risque de population :

$$
L_D(\theta, g)
$$

où $g_0$ est un **paramètre de nuisance** inconnu (score de propension, fonction de régression...).

**Défi :** Minimiser l'**excès de risque** suivant, même lorsque $g_0$ doit être estimé à partir des données :

$$
L_D(\hat{\theta}, g_0) - L_D(\theta^*, g_0)
$$

---

## Défis principaux

- **Propagation d'erreur** : Les erreurs d'estimation sur $g$ contaminent fortement l'estimation de $\theta$  
- **Complexité des modèles** : Les garanties classiques échouent avec des estimateurs non-paramétriques  
- **Échantillons finis** : Les garanties asymptotiques sont insuffisantes en pratique

---

## Contributions clés

### 1. Orthogonalité de Neyman

Condition cruciale :

$$
D_g D_\theta L_D(\theta^*, g_0)[\theta - \theta^*, g - g_0] = 0
$$

 Cette condition découple l'erreur sur $\hat{g}$ de celle sur $\hat{\theta}$.

**Effet :** L’erreur sur $\hat{g}$ a un **impact d’ordre supérieur**, typiquement :

$$
\|\hat{g} - g_0\|^4 \quad \text{au lieu de} \quad \|\hat{g} - g_0\|
$$

---

### 2. Méta-algorithme en deux étapes

1. Estimer $\hat{g}$ sur un sous-échantillon $S_1$  
2. Estimer $\hat{\theta}$ sur $S_2$ avec une perte orthogonale

 Cela évite le surajustement et permet une analyse indépendante des deux étapes.

---

### 3. Taux oracle

Si $\|\hat{g} - g_0\| = o(n^{-1/4})$, alors :

$$
L_D(\hat{\theta}, g_0) - L_D(\theta^*, g_0) = O(1/n)
$$

 On obtient les **mêmes performances** que si $g_0$ était connu.

---

## Exemples d’application

### Estimation d'effets de traitement hétérogènes (CATE)

- **Perte R-Loss** :
  $$
  \ell(\theta, g) = (Y - q(W) - \theta(X)(T - e(W)))^2
  $$
- **Résultat** : Taux optimal si $\|\hat{g} - g_0\| = O(n^{-1/4})$

---

### Apprentissage de politiques (Policy Learning)

- **Perte DR-Loss** : doublement robuste
- **But** : apprendre une politique binaire optimale $\theta(X) \in \{0, 1\}$

---

## Avantages

- **Flexible** : compatible avec n'importe quel algorithme (forêts, réseaux...)
- **Robuste** : fonctionne même si $\theta$ est mal spécifié
- **Domaines d’application** : santé, économie, données manquantes, adaptation de domaine...

---

## Limitations

- Nécessite une **division d’échantillon** (ou cross-fitting)
- La **construction d’une perte orthogonale** adaptée peut être non-triviale
- L’**orthogonalité doit être vérifiée** pour chaque nouveau problème
