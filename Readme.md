# 🧠 Orthogonal Statistical Learning — Résumé synthétique

Ce dépôt résume le papier **"Orthogonal Statistical Learning"** (Foster & Syrgkanis), qui traite de l'estimation de paramètres causaux dans des contextes où la fonction de risque dépend de **paramètres de nuisance** inconnus.

---

## 🎯 Objectif

Estimer un **paramètre cible** `θ` (ex. : effet d’un traitement, politique optimale) à partir d’un risque :

$L_D(\theta, g)$


où `g₀` est un **paramètre de nuisance** (propension, régression…). On souhaite garantir que l’**excès de risque** :

L_D(θ̂, g₀) − L_D(θ*, g₀)


reste faible, même si `g₀` est estimé à partir des données et donc imparfait.

---

## ⚠️ Défis

- **Erreur sur g** : contamine fortement θ̂ si mal estimé.
- **Modèles complexes** : les garanties classiques échouent avec des modèles non-paramétriques.
- **Robustesse** : les garanties asymptotiques ne tiennent pas en échantillons finis.

---

## ✅ Contributions

### 1. **Orthogonalité de Neyman**
Permet de découpler les erreurs sur `g` et `θ`.  
Effet : l’erreur sur `ĝ` a un **impact d’ordre supérieur** (ex. ∥ĝ−g₀∥⁴).

### 2. **Méthode en deux étapes**
- Étape 1 : estimer `ĝ` sur un sous-échantillon.
- Étape 2 : estimer `θ̂` avec une perte orthogonale sur l’autre moitié.

### 3. **Taux oracle**
Si `ĝ` est assez précis, `θ̂` atteint les mêmes performances que si `g₀` était connu.

---

## 📚 Exemples

### ▶️ Estimation d’effet de traitement (CATE)
- Perte : **R-Loss**  
ℓ(θ, g) = (Y − q(W) − θ(X)(T − e(W)))²  
→ Si ∥ĝ−g₀∥ = o(n⁻¹/⁴), alors taux optimal sur θ̂.

### ▶️ Apprentissage de politiques
- Perte : **DR-Loss**  
Permet d’optimiser une politique binaire θ(X) ∈ {0,1} malgré les biais.

---

## 💡 Atouts

- Fonctionne avec **tout algorithme ML** (réseaux, forêts…)
- Valide même si le modèle est mal spécifié
- Applications : santé, économie, données manquantes, etc.

---

## ⚠️ Limites

- Requiert un **split d’échantillon** (ou cross-fitting)
- L’**orthogonalité** doit être vérifiée pour chaque tâche

---

## 📄 Référence

Foster, D. & Syrgkanis, V. (2023). *Orthogonal Statistical Learning*. [arXiv:1901.09036](https://arxiv.org/abs/1901.09036)

