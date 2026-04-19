# 🐼 Pandas : Le Vrai Remplacement d'Excel

> *"Excel est un tableur. Pandas est un laboratoire de données."*

---

## 🎯 Pourquoi Pandas écrase Excel

Vous avez survécu aux bases de Python. Maintenant, place à l'outil qui va **changer votre vie** (rien que ça).

**Pandas** est une bibliothèque Python qui introduit deux structures magiques :

- **`Series`** : une colonne de données.
- **`DataFrame`** : un tableau 2D (lignes × colonnes), l'équivalent d'une feuille Excel sous stéroïdes.

Avec Pandas, vous pouvez :

- 📥 Charger des fichiers CSV, Excel, JSON, SQL en **une ligne**.
- 🧹 Nettoyer les données (doublons, valeurs manquantes) en **quelques secondes**.
- 🔍 Filtrer, trier, grouper avec une syntaxe **lisible**.
- 📊 Agréger des millions de lignes **sans ramer**.

!!! success "Objectif de ce chapitre"
    À la fin, vous saurez remplacer 90% de vos tâches Excel par du code Python. Et vous ne voudrez plus jamais ouvrir un fichier de 500 Mo qui rame.

---

## 📥 1. Charger des données

Avant toute chose, importons Pandas (l'alias `pd` est universel).

```python
import pandas as pd
```

### Depuis un fichier CSV
```python
df = pd.read_csv('data/ventes.csv')
```

### Depuis un fichier Excel
```python
df = pd.read_excel('data/rapport.xlsx', sheet_name='Feuil1')
```

### Aperçu rapide
```python
df.head()      # 5 premières lignes
df.tail(3)     # 3 dernières lignes
df.shape       # (nb_lignes, nb_colonnes)
df.info()      # Types des colonnes et valeurs manquantes
df.describe()  # Statistiques descriptives (count, mean, min, max...)
```

!!! tip "Astuce"
    Utilisez `df.head()` systématiquement pour vérifier que le chargement s'est bien passé.

---

## 🧹 2. Nettoyer les données (le vrai travail)

En Data Analysis, on passe **80% du temps à nettoyer**. Pandas rend cette étape presque plaisante.

### Supprimer les doublons
```python
df.drop_duplicates(inplace=True)
```

### Gérer les valeurs manquantes
```python
# Supprimer les lignes avec au moins une valeur vide
df.dropna(inplace=True)

# Remplacer les valeurs manquantes par une valeur par défaut
df.fillna(0, inplace=True)
df['colonne'].fillna(df['colonne'].mean(), inplace=True)
```

### Renommer des colonnes
```python
df.rename(columns={'ancien_nom': 'nouveau_nom', 'Qté': 'Quantité'}, inplace=True)
```

### Changer le type d'une colonne
```python
df['Date'] = pd.to_datetime(df['Date'])
df['Prix'] = df['Prix'].astype(float)
```

---

## 🔍 3. Filtrer et Trier (comme Excel, en plus puissant)

### Filtrer les lignes selon une condition
```python
# Ventes > 1000
df[df['Montant'] > 1000]

# Clients de Kinshasa
df[df['Ville'] == 'Kinshasa']

# Plusieurs conditions
df[(df['Montant'] > 500) & (df['Ville'] == 'Lubumbashi')]
```

### Trier les données
```python
# Tri croissant par âge
df.sort_values('Age', ascending=True, inplace=True)

# Tri décroissant par montant, puis par date
df.sort_values(['Montant', 'Date'], ascending=[False, True], inplace=True)
```

### Sélectionner des colonnes spécifiques
```python
df[['Nom', 'Montant']]  # Garde uniquement ces deux colonnes
```

---

## 📊 4. Agréger et Grouper (les Tableaux Croisés Dynamiques)

C'est ici que Pandas écrase littéralement Excel.

### `groupby()` : l'équivalent des TCD
```python
# Chiffre d'affaires total par ville
ca_par_ville = df.groupby('Ville')['Montant'].sum()

# Moyenne des montants par catégorie de produit
df.groupby('Catégorie')['Montant'].mean()

# Plusieurs agrégations à la fois
df.groupby('Ville').agg({
    'Montant': 'sum',
    'Client': 'count',
    'Note': 'mean'
})
```

### `value_counts()` : compter les occurrences
```python
# Nombre de commandes par client
df['Client'].value_counts()
```

### `pivot_table()` : pour les croisés dynamiques avancés
```python
pd.pivot_table(df, values='Montant', index='Ville', columns='Mois', aggfunc='sum', fill_value=0)
```

!!! info "Performance"
    Ces opérations sur un million de lignes prennent **moins d'une seconde** avec Pandas. Excel aurait déjà crashé.

---

## 🧪 5. Exemple concret : Analyse de ventes

Reprenons notre mini-base de la Partie 2 et utilisons Pandas.

```python
import pandas as pd

# Création manuelle d'un DataFrame (équivalent à une feuille Excel)
data = {
    'Produit': ['Chaise', 'Table', 'Lampe', 'Chaise', 'Table'],
    'Quantité': [4, 1, 3, 2, 2],
    'Prix_Unitaire': [45, 150, 25, 45, 150]
}
df = pd.DataFrame(data)

# Calcul du chiffre d'affaires par ligne
df['CA'] = df['Quantité'] * df['Prix_Unitaire']

# CA total
ca_total = df['CA'].sum()
print(f"Chiffre d'affaires total : {ca_total} €")

# Top produits par CA
top_produits = df.groupby('Produit')['CA'].sum().sort_values(ascending=False)
print(top_produits)
```

**Résultat :**
```
Chiffre d'affaires total : 735 €
Produit
Table     450
Chaise    270
Lampe      75
Name: CA, dtype: int64
```

Avouez que c'est plus élégant qu'une formule `=SOMME.SI.ENS()` à rallonge. 😎

---

## 🔁 6. Jointures : comme des RECHERCHEV() sous stéroïdes

Fusionner deux tables est un jeu d'enfant avec `merge()`.

```python
clients = pd.DataFrame({
    'Client_ID': [1, 2, 3],
    'Nom': ['Alice', 'Bob', 'Charlie']
})

commandes = pd.DataFrame({
    'Commande_ID': [101, 102, 103],
    'Client_ID': [2, 1, 2],
    'Montant': [120, 75, 200]
})

# Jointure pour avoir le nom du client dans les commandes
fusion = pd.merge(commandes, clients, on='Client_ID', how='left')
print(fusion)
```

| Commande_ID | Client_ID | Montant | Nom     |
|-------------|-----------|---------|---------|
| 101         | 2         | 120     | Bob     |
| 102         | 1         | 75      | Alice   |
| 103         | 2         | 200     | Bob     |

---

## ✅ Check-list du Data Analyst Pandas

- [ ] Charger un fichier CSV / Excel avec `pd.read_csv()`
- [ ] Explorer rapidement avec `.head()`, `.info()`, `.describe()`
- [ ] Nettoyer : `drop_duplicates()`, `dropna()`, `fillna()`
- [ ] Filtrer avec des conditions booléennes
- [ ] Trier avec `sort_values()`
- [ ] Agréger avec `groupby()` et `agg()`
- [ ] Fusionner avec `merge()`

---

## 🔜 Prochaine étape

Vous maîtrisez maintenant l'analyse de données. Mais pour **communiquer** vos résultats, rien ne vaut un beau graphique. C'est l'objet de la **Partie 3 : La Visualisation**.


---

*Fun fact : Wes McKinney, le créateur de Pandas, a commencé ce projet dans un hedge fund pour analyser des données financières. Aujourd'hui, c'est l'outil le plus utilisé en Data Science.* 📈
```

Ce fichier est complet et cohérent avec les précédents. Souhaitez-vous que nous enchaînions avec la page d'introduction de la **Partie 3 - Visualisation** ?