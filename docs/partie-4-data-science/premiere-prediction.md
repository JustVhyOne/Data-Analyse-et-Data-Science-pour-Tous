# 🌸 Votre première prédiction

> *"Parler de Machine Learning, c'est bien. En faire, c'est mieux."*

---

## 🎯 Objectif de ce chapitre

Vous allez entraîner votre **tout premier modèle de Machine Learning**. Nous utiliserons le célèbre **dataset Iris**, le "Hello World" de la Data Science.

À la fin de ce chapitre, vous aurez :

- ✅ Chargé et exploré des données réelles.
- ✅ Séparé les données en entraînement et test.
- ✅ Entraîné un **Arbre de Décision**.
- ✅ Évalué sa précision.
- ✅ Fait une prédiction sur une nouvelle fleur inconnue.

**Temps estimé** : 15 minutes. **Prérequis** : Un notebook Google Colab ouvert.

---

## 📦 1. Importer les bibliothèques

```python
import pandas as pd
import numpy as np
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.metrics import accuracy_score, classification_report
import matplotlib.pyplot as plt

# Pour un affichage plus joli
plt.style.use('seaborn-v0_8-whitegrid')
```

---

## 🌸 2. Charger et découvrir le dataset Iris

Le dataset Iris contient 150 fleurs, réparties en 3 espèces (Setosa, Versicolor, Virginica). Pour chaque fleur, on a 4 mesures :

- Longueur du sépale (`sepal length`)
- Largeur du sépale (`sepal width`)
- Longueur du pétale (`petal length`)
- Largeur du pétale (`petal width`)

```python
# Charger le dataset
iris = load_iris()
X = pd.DataFrame(iris.data, columns=iris.feature_names)
y = pd.Series(iris.target, name='species')

# Aperçu des features
print("Features (X) :")
display(X.head())

print("\nTarget (y) :")
display(y.head())

# Les noms des espèces
print("\nEspèces :", iris.target_names)
# 0 = setosa, 1 = versicolor, 2 = virginica
```

| sepal length (cm) | sepal width (cm) | petal length (cm) | petal width (cm) |
|-------------------|------------------|-------------------|------------------|
| 5.1               | 3.5              | 1.4               | 0.2              |
| 4.9               | 3.0              | 1.4               | 0.2              |

---

## 🔍 3. Exploration rapide

```python
# Dimensions
print(f"Nombre de fleurs : {X.shape[0]}")
print(f"Nombre de features : {X.shape[1]}")

# Répartition des espèces
print("\nRépartition :")
print(y.value_counts().map({0: 'setosa', 1: 'versicolor', 2: 'virginica'}))
```

```
Nombre de fleurs : 150
Nombre de features : 4

Répartition :
setosa        50
versicolor    50
virginica     50
```

Le dataset est parfaitement **équilibré** : 50 fleurs de chaque espèce.

---

## ✂️ 4. Séparer les données en Entraînement et Test

On garde 30% des données pour le test.

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, 
    test_size=0.3,        # 30% pour le test
    random_state=42,      # Pour des résultats reproductibles
    stratify=y            # Garde la même proportion d'espèces dans train et test
)

print(f"Taille entraînement : {X_train.shape[0]} fleurs")
print(f"Taille test : {X_test.shape[0]} fleurs")
```

```
Taille entraînement : 105 fleurs
Taille test : 45 fleurs
```

!!! tip "À quoi sert `stratify=y` ?"
    Sans `stratify`, on pourrait avoir par hasard plus de setosa dans le test que dans l'entraînement. `stratify` garantit que la répartition des espèces est identique dans les deux ensembles.

---

## 🌳 5. Créer et entraîner l'Arbre de Décision

```python
# Créer le modèle
model = DecisionTreeClassifier(
    max_depth=3,          # Limiter la profondeur pour éviter le surapprentissage
    random_state=42
)

# Entraîner (c'est ici que "l'apprentissage" a lieu)
model.fit(X_train, y_train)

print("✅ Modèle entraîné avec succès !")
```

**Explication** : En une ligne (`model.fit()`), l'algorithme a analysé les 105 fleurs d'entraînement et construit un arbre de décision pour séparer les espèces.

---

## 📊 6. Visualiser l'arbre de décision

C'est l'avantage de l'arbre : on peut **voir** comment il raisonne.

```python
plt.figure(figsize=(12, 8))
plot_tree(
    model,
    feature_names=iris.feature_names,
    class_names=iris.target_names.tolist(),
    filled=True,
    rounded=True,
    fontsize=10
)
plt.title("Arbre de Décision - Dataset Iris", fontsize=16, fontweight='bold')
plt.show()
```

**Lecture de l'arbre** :
- Le premier nœud (tout en haut) regarde si `petal length (cm) <= 2.45`.
- Si OUI → on va à gauche → c'est une **setosa**.
- Si NON → on va à droite → on continue avec `petal width (cm)`, etc.

---

## 📈 7. Évaluer le modèle sur les données de test

```python
# Prédire sur les données de test
y_pred = model.predict(X_test)

# Calculer l'accuracy
accuracy = accuracy_score(y_test, y_pred)
print(f"✅ Accuracy : {accuracy:.2%}")

# Rapport détaillé
print("\nRapport de classification :")
print(classification_report(y_test, y_pred, target_names=iris.target_names))
```

```
✅ Accuracy : 95.56%

Rapport de classification :
              precision    recall  f1-score   support

      setosa       1.00      1.00      1.00        15
  versicolor       0.94      0.94      0.94        16
   virginica       0.93      0.93      0.93        14

    accuracy                           0.96        45
   macro avg       0.96      0.96      0.96        45
weighted avg       0.96      0.96      0.96        45
```

**Interprétation** :
- **Accuracy globale** : 95.6% des 45 fleurs de test ont été correctement classées.
- **Setosa** : 100% de réussite (facile à reconnaître).
- **Versicolor et Virginica** : une petite confusion (une fleur mal classée dans chaque catégorie).

Pour un premier modèle, c'est **excellent** !

---

## 🔮 8. Prédire sur une nouvelle fleur inconnue

Imaginons qu'on découvre une nouvelle fleur avec les mesures suivantes :

- Longueur du sépale : 5.8 cm
- Largeur du sépale : 2.7 cm
- Longueur du pétale : 4.1 cm
- Largeur du pétale : 1.0 cm

```python
# Créer un DataFrame pour la nouvelle fleur
nouvelle_fleur = pd.DataFrame({
    'sepal length (cm)': [5.8],
    'sepal width (cm)': [2.7],
    'petal length (cm)': [4.1],
    'petal width (cm)': [1.0]
})

# Prédire l'espèce
prediction = model.predict(nouvelle_fleur)
probabilites = model.predict_proba(nouvelle_fleur)

espece = iris.target_names[prediction[0]]
print(f"🌸 Espèce prédite : **{espece}**")

# Afficher les probabilités
for i, espece_name in enumerate(iris.target_names):
    print(f"   {espece_name} : {probabilites[0][i]:.2%}")
```

```
🌸 Espèce prédite : **versicolor**

   setosa : 0.00%
   versicolor : 100.00%
   virginica : 0.00%
```

Le modèle est **certain à 100%** que c'est une Iris Versicolor. 🎯

---

## 🧪 9. Exercice : Testez vous-même !

Essayez avec ces autres mesures :

| sepal length | sepal width | petal length | petal width |
|--------------|-------------|--------------|-------------|
| 6.3          | 3.3         | 6.0          | 2.5         |
| 4.9          | 3.1         | 1.5          | 0.1         |

Quelle espèce prédit le modèle pour chacune ?

!!! success "Réponse (à vérifier après avoir essayé)"
    Première fleur → **virginica** (grands pétales).  
    Deuxième fleur → **setosa** (petits pétales).

---

## 💾 10. Sauvegarder votre modèle (bonus)

Pour réutiliser votre modèle plus tard sans le réentraîner :

```python
import joblib

# Sauvegarder
joblib.dump(model, 'mon_premier_modele_iris.pkl')

# Charger plus tard
# model_charge = joblib.load('mon_premier_modele_iris.pkl')
```

---

## ✅ Check-list de votre premier projet ML

- [ ] Charger des données avec Scikit-learn
- [ ] Séparer train/test avec `train_test_split`
- [ ] Choisir un modèle (`DecisionTreeClassifier`)
- [ ] Entraîner avec `.fit()`
- [ ] Prédire avec `.predict()`
- [ ] Évaluer avec `accuracy_score()`
- [ ] Prédire sur de nouvelles données

---

## 🎉 Félicitations !

Vous venez de réaliser votre **première analyse de Machine Learning complète**. 

Ce que vous avez appris ici est **la base de tout projet de Data Science** :
1. Préparer les données
2. Entraîner un modèle
3. Évaluer
4. Prédire

Dans la prochaine et dernière partie, nous verrons des outils essentiels pour **travailler comme un pro** : environnement virtuel et Git.


---

*Fun fact : Le dataset Iris a été collecté par Edgar Anderson en 1935 sur la péninsule de Gaspé au Canada. Ces fleurs sont devenues les stars de la Data Science.* 🌸