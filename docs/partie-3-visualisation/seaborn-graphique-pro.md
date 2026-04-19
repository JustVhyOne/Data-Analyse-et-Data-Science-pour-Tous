# 📈 Graphique Pro avec Seaborn

> *"Avec Seaborn, un graphique moche est plus difficile à faire qu'un beau graphique."*

---

## 🎯 Qu'est-ce que Seaborn ?

**Seaborn** est une bibliothèque Python de visualisation statistique basée sur **Matplotlib**. Elle a été conçue pour :

- 🎨 Produire des graphiques **esthétiques par défaut** (palettes de couleurs harmonieuses, grilles élégantes).
- 📊 Simplifier les visualisations statistiques complexes (distributions, régressions, catégories).
- 🤝 S'intégrer **parfaitement** avec les DataFrames Pandas.

!!! success "Objectif du chapitre"
    À la fin, vous saurez transformer un DataFrame brut en un graphique digne d'un rapport professionnel en **deux lignes de code**.

---

## 📦 Installation et import

Dans Google Colab, Seaborn est déjà installé. Il suffit de l'importer :

```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd

# Pour que les graphiques s'affichent dans le notebook
%matplotlib inline
```

!!! tip "Jeu de données intégré"
    Seaborn embarque plusieurs jeux de données d'exemple. Nous utiliserons `tips` (pourboires dans un restaurant) et `penguins` (manchots) pour les démonstrations.

```python
# Charger un dataset exemple
tips = sns.load_dataset('tips')
tips.head()
```

| total_bill | tip  | sex    | smoker | day | time   | size |
|------------|------|--------|--------|-----|--------|------|
| 16.99      | 1.01 | Female | No     | Sun | Dinner | 2    |
| 10.34      | 1.66 | Male   | No     | Sun | Dinner | 3    |

---

## 🧱 1. Le B.A.-BA : `sns.set_style()` et `sns.set_palette()`

Avant de tracer, on peut définir le **style global**.

```python
# Styles disponibles : darkgrid, whitegrid, dark, white, ticks
sns.set_style("whitegrid")

# Palette de couleurs : deep, muted, bright, pastel, dark, colorblind
sns.set_palette("viridis")
```

---

## 📊 2. Graphiques de distribution (une seule variable)

### Histogramme
```python
sns.histplot(data=tips, x='total_bill', bins=30, kde=True)
plt.title("Distribution du montant total des additions")
plt.show()
```
- `bins` : nombre de barres.
- `kde=True` : ajoute une courbe de densité lissée.

### Boxplot (boîte à moustaches)
Idéal pour repérer les valeurs aberrantes et la dispersion.

```python
sns.boxplot(data=tips, x='day', y='total_bill')
plt.title("Montant total par jour de la semaine")
plt.show()
```

### Violinplot
Combine boxplot et densité.

```python
sns.violinplot(data=tips, x='day', y='total_bill', palette='muted')
plt.title("Distribution du montant total par jour")
plt.show()
```

---

## 📈 3. Graphiques de relation (deux variables)

### Nuage de points (scatter plot)
```python
sns.scatterplot(data=tips, x='total_bill', y='tip', hue='sex', style='smoker', size='size')
plt.title("Relation entre addition et pourboire")
plt.show()
```
- `hue` : couleur selon une variable catégorielle.
- `style` : forme des points.
- `size` : taille des points.

### Ligne (line plot) – idéal pour les séries temporelles
```python
# Exemple avec un dataset de vols
flights = sns.load_dataset('flights')
flights_pivot = flights.pivot(index='month', columns='year', values='passengers')

sns.lineplot(data=flights_pivot)
plt.title("Évolution du nombre de passagers par mois")
plt.xticks(rotation=45)
plt.show()
```

### Régression linéaire (`regplot` et `lmplot`)
Ajoute automatiquement une droite de régression avec intervalle de confiance.

```python
sns.lmplot(data=tips, x='total_bill', y='tip', hue='sex', height=5, aspect=1.2)
plt.title("Régression linéaire du pourboire en fonction de l'addition")
plt.show()
```

!!! info "Différence `regplot` vs `lmplot`"
    `regplot` trace un seul graphique. `lmplot` combine `regplot` avec `FacetGrid` pour créer des sous-graphiques par catégorie (ex: une régression pour les hommes, une pour les femmes).

---

## 🧩 4. Graphiques catégoriels (comparer des groupes)

### Barplot (moyenne + barre d'erreur)
```python
sns.barplot(data=tips, x='day', y='total_bill', hue='sex', ci='sd')
plt.title("Addition moyenne par jour et par sexe")
plt.show()
```
- `ci='sd'` : affiche l'écart-type au lieu de l'intervalle de confiance à 95%.

### Countplot (compter les occurrences)
```python
sns.countplot(data=tips, x='day', hue='time')
plt.title("Nombre de repas par jour et par service")
plt.show()
```

### Pointplot (moyenne avec points)
```python
sns.pointplot(data=tips, x='day', y='total_bill', hue='sex', dodge=True, markers=['o', 's'])
plt.title("Évolution de l'addition moyenne")
plt.show()
```

---

## 🎨 5. Personnalisation avancée

### Changer la taille de la figure
```python
plt.figure(figsize=(12, 6))
sns.scatterplot(data=tips, x='total_bill', y='tip')
plt.show()
```

### Ajouter des titres et labels
```python
sns.histplot(data=tips, x='total_bill')
plt.title("Distribution des additions", fontsize=16, fontweight='bold')
plt.xlabel("Montant total ($)")
plt.ylabel("Nombre de repas")
plt.show()
```

### Utiliser un thème temporaire
```python
with sns.axes_style("darkgrid"):
    sns.scatterplot(data=tips, x='total_bill', y='tip')
    plt.show()
```

---

## 🧪 6. Exemple complet : Analyse des manchots

```python
# Chargement
penguins = sns.load_dataset('penguins').dropna()

# Style
sns.set_style("whitegrid")
sns.set_palette("Set2")

# Création d'une figure à plusieurs sous-graphiques
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# 1. Distribution de la masse corporelle par espèce
sns.histplot(data=penguins, x='body_mass_g', hue='species', kde=True, ax=axes[0, 0])
axes[0, 0].set_title("Distribution de la masse corporelle")

# 2. Relation longueur des nageoires vs masse
sns.scatterplot(data=penguins, x='flipper_length_mm', y='body_mass_g', hue='species', style='sex', ax=axes[0, 1])
axes[0, 1].set_title("Nageoires vs Masse")

# 3. Boxplot de la longueur du bec par île
sns.boxplot(data=penguins, x='island', y='bill_length_mm', hue='species', ax=axes[1, 0])
axes[1, 0].set_title("Longueur du bec par île")

# 4. Countplot des espèces par île
sns.countplot(data=penguins, x='island', hue='species', ax=axes[1, 1])
axes[1, 1].set_title("Répartition des espèces par île")

plt.tight_layout()
plt.show()
```

---

## 📤 7. Sauvegarder un graphique

```python
sns.scatterplot(data=tips, x='total_bill', y='tip')
plt.title("Pourboires vs Addition")
plt.savefig('mon_graphique.png', dpi=300, bbox_inches='tight')
plt.show()
```
- `dpi` : résolution (300 pour l'impression).
- `bbox_inches='tight'` : supprime les marges inutiles.

---

## ✅ Check-list du Visualiseur Seaborn

- [ ] Importer Seaborn et définir un style
- [ ] Tracer un histogramme / boxplot
- [ ] Tracer un scatterplot avec `hue`
- [ ] Tracer un barplot ou countplot
- [ ] Personnaliser titres et labels
- [ ] Sauvegarder le graphique en haute résolution

---

## 🔜 Prochain chapitre

Vous maîtrisez maintenant les graphiques **statiques** professionnels. Mais si vous voulez impressionner avec des **animations** et des **interactions**, c'est Plotly qu'il vous faut.


---

*Fun fact : Michael Waskom, le créateur de Seaborn, est neuroscientifique. Il a créé cette bibliothèque pour visualiser des données cérébrales.* 🧠