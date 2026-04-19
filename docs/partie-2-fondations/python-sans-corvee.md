# 🐣 Python sans la corvée

> *Apprendre Python pour la Data, c'est comme apprendre à cuisiner : pas besoin de savoir dépecer un bœuf pour faire une omelette.*

---

## 🎯 L'essentiel en 30 minutes

Python est un langage vaste. Heureusement, pour faire de l'analyse de données, vous n'avez besoin que d'une **poignée de concepts**. Ce chapitre vous les présente avec des exemples concrets.

!!! info "Prérequis"
    Ayez un notebook Google Colab ouvert (cf. Partie 1). Tous les exemples de code sont exécutables directement.

---

## 🧱 1. Les variables et les types

Une **variable** est une boîte dans laquelle on stocke une valeur. En Python, pas besoin de déclarer le type, il est déduit automatiquement.

```python
# Les types de base
age = 25                  # int (entier)
prix_moyen = 19.99        # float (décimal)
nom = "Justicier"         # str (chaîne de caractères)
est_abonne = True         # bool (booléen : True ou False)
```

### 🔄 Conversion de types
Parfois, on doit convertir (par exemple, un nombre lu dans un fichier texte est souvent une `str`).

```python
nombre_texte = "42"
vrai_nombre = int(nombre_texte)   # -> 42
prix = "19.99"
prix_float = float(prix)          # -> 19.99
```

---

## 📋 2. Les structures de données essentielles

### Listes
Une collection **ordonnée** d'éléments, modifiable.

```python
fruits = ["pomme", "banane", "cerise"]
notes = [14, 16, 12, 18]

# Accès par index (commence à 0)
premier_fruit = fruits[0]   # "pomme"

# Ajouter un élément
fruits.append("orange")

# Longueur
nb_fruits = len(fruits)     # 4
```

### Dictionnaires
Une collection **clé-valeur**, parfaite pour représenter des enregistrements.

```python
client = {
    "nom": "Mabiala",
    "age": 34,
    "ville": "Kinshasa",
    "montant_achats": 1250.50
}

# Accès par clé
print(client["nom"])        # "Mabiala"
print(client.get("email", "Non renseigné"))  # Évite l'erreur si la clé n'existe pas

# Ajouter / Modifier
client["email"] = "mabiala@email.com"
```

!!! tip "Pourquoi les dictionnaires sont rois en Data ?"
    Chaque ligne d'un fichier CSV peut être vue comme un dictionnaire. Pandas les utilise massivement en interne.

---

## 🔁 3. Les boucles (pour ne pas se répéter)

### Boucle `for` sur une liste
```python
prenoms = ["Alice", "Bob", "Charlie"]
for p in prenoms:
    print(f"Bonjour {p} !")
```

### Boucle `for` avec `range()`
Pour répéter une action un certain nombre de fois.

```python
for i in range(5):   # 0, 1, 2, 3, 4
    print(f"Itération n°{i}")
```

### Boucle `for` sur un dictionnaire
```python
for cle, valeur in client.items():
    print(f"{cle} : {valeur}")
```

---

## ⚙️ 4. Les fonctions (écrire une fois, utiliser partout)

Une fonction est un bloc de code réutilisable. On la définit avec `def`.

```python
def calculer_ttc(prix_ht, taux_tva=0.20):
    """
    Calcule le prix TTC.
    Si aucun taux n'est fourni, 20% est utilisé par défaut.
    """
    return prix_ht * (1 + taux_tva)

# Utilisation
prix_final = calculer_ttc(100)        # 120.0
prix_reduit = calculer_ttc(100, 0.055) # 105.5 (TVA à 5.5%)
```

!!! success "L'avantage des fonctions"
    Vous écrivez la logique une seule fois. Si la formule change, vous la modifiez à un seul endroit.

---

## 🧪 5. Conditions (prendre des décisions)

```python
age = 17
if age >= 18:
    print("Accès autorisé")
elif age >= 16:
    print("Accès accompagné uniquement")
else:
    print("Accès interdit")
```

**Opérateurs de comparaison utiles :**
- `==` égal à
- `!=` différent de
- `<` , `<=` , `>` , `>=`
- `and` , `or` , `not`

---

## 📦 6. Importer des modules (la force de Python)

La puissance de Python réside dans ses **bibliothèques**. Pour les utiliser, on les importe.

```python
import pandas as pd          # pd est un alias standard
import numpy as np           # np aussi
import matplotlib.pyplot as plt

# Maintenant on peut utiliser pd.read_csv(), np.mean(), plt.plot(), etc.
```

---

## 🧠 Mise en pratique : Petit exercice avant Pandas

Imaginons une mini-base de données de ventes sous forme de liste de dictionnaires.

```python
ventes = [
    {"produit": "Chaise", "quantite": 4, "prix_unitaire": 45},
    {"produit": "Table", "quantite": 1, "prix_unitaire": 150},
    {"produit": "Lampe", "quantite": 3, "prix_unitaire": 25}
]
```

**Objectif** : Calculer le chiffre d'affaires total.

```python
ca_total = 0
for vente in ventes:
    ca_produit = vente["quantite"] * vente["prix_unitaire"]
    ca_total += ca_produit

print(f"Chiffre d'affaires total : {ca_total} €")   # 405 €
```

Avec Pandas, ce genre d'opération se fera en une ligne. Mais vous comprenez maintenant ce qui se passe sous le capot. 🔧

---

## ✅ Check-list du Data Analyst Python

À la fin de ce chapitre, assurez-vous de maîtriser :

- [ ] Créer une variable et connaître son type
- [ ] Manipuler une liste (ajout, accès)
- [ ] Utiliser un dictionnaire (clé-valeur)
- [ ] Écrire une boucle `for` simple
- [ ] Définir une fonction basique
- [ ] Utiliser une condition `if/else`
- [ ] Importer une bibliothèque

Si c'est bon, **vous êtes prêt pour Pandas** !

---

## 🔜 Prochain chapitre

Maintenant que vous avez les bases du langage, attaquons le cœur du sujet : **Pandas**, la bibliothèque qui va définitivement remplacer Excel dans votre boîte à outils.


---

*Fun fact : "Python" ne vient pas du serpent, mais des Monty Python, un groupe d'humoristes britanniques. D'où les noms "Spam", "Eggs" dans les exemples officiels.* 🥚
```

Ce fichier est complet et prêt à être utilisé. Voulez-vous poursuivre avec le chapitre suivant `pandas-excel-killer.md` ?