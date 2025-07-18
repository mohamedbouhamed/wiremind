# Orthogonal Statistical Learning — Résumé synthétique

Ce dépôt résume l'article **"Orthogonal Statistical Learning"** (Foster & Syrgkanis), qui traite de l'estimation robuste de paramètres causaux en présence de **paramètres de nuisance** inconnus.

---

## Objectif

Estimer un **paramètre cible** $\theta$ (ex. : effet de traitement, politique optimale) à partir d'un risque de population :

$$L_D(\theta, g)$$

où $g_0$ est un **paramètre de nuisance** inconnu (score de propension, fonction de régression...). 

**Défi :** Minimiser l'**excès de risque** suivant, même lorsque $g_0$ doit être estimé à partir des données :

$$L_D(\hat{\theta}, g_0) - L_D(\theta^*, g_0)$$

---

## Défis principaux

- **Propagation d'erreur** : Les erreurs d'estimation sur $g$ contaminent fortement l'estimation de $\theta$
- **Complexité des modèles** : Les garanties classiques échouent avec des modèles non-paramétriques 
- **Échantillons finis** : Les garanties asymptotiques ne sont pas suffisantes en pratique

---

## Contributions clés

### 1. **Condition d'orthogonalité de Neyman**
Découple les erreurs d'estimation sur $g$ et $\theta$ en imposant que la dérivée croisée du risque s'annule :

$$D_g D_\theta L_D(\theta^*, g_0)[\theta - \theta^*, g - g_0] = 0$$

**Effet :** L'erreur sur $\hat{g}$ n'a qu'un **impact d'ordre supérieur** (ex. : $\|\hat{g} - g_0\|^4$ au lieu de $\|\hat{g} - g_0\|$).

### 2. **Méta-algorithme en deux étapes**
- **Étape 1 :** Estimer $\hat{g}$ sur un sous-échantillon $S_1$
- **Étape 2 :** Estimer $\hat{\theta}$ avec une fonction de perte orthogonale sur $S_2$

### 3. **Taux oracle**
Si $\hat{g}$ converge suffisamment vite (typiquement $n^{-1/4}$), alors $\hat{\theta}$ atteint les mêmes performances que si $g_0$ était parfaitement connu.

---

## Exemples d'application

###  **Estimation d'effets de traitement hétérogènes (CATE)**
- **Perte R-Loss :**  
  $$\ell(\theta, g) = (Y - q(W) - \theta(X)(T - e(W)))^2$$
- **Garantie :** Si $\|\hat{g} - g_0\| = O(n^{-1/4})$, alors taux optimal pour $\hat{\theta}$

### **Apprentissage de politiques (Policy Learning)**
- **Perte DR-Loss doublement robuste**
- **Application :** Optimisation de politiques de traitement binaires $\theta(X) \in \{0,1\}$

---

## Avantages

- ✅ **Flexibilité algorithmique** : Compatible avec tout algorithme ML (réseaux de neurones, forêts aléatoires...)
- ✅ **Robustesse** : Fonctionne même si le modèle est mal spécifié
- ✅ **Applications variées** : Santé publique, économie, données manquantes, adaptation de domaine

---

## Limitations

- **Division d'échantillon** requise (ou cross-fitting pour plus d'efficacité)
- **Vérification de l'orthogonalité** nécessaire pour chaque nouvelle application
- **Construction des pertes orthogonales** peut être non-triviale


