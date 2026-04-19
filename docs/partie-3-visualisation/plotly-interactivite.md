# 🎮 Interactivité & Animations avec Plotly

> *"Avec Plotly, vos données ne sont plus des images figées : elles prennent vie sous les yeux de votre audience."*

---

## 🎯 Qu'est-ce que Plotly ?

**Plotly** est une bibliothèque de visualisation qui génère des graphiques **interactifs**. Contrairement à Matplotlib ou Seaborn qui produisent des images statiques (PNG, JPG), Plotly crée du **HTML + JavaScript**.

Cela signifie que vos graphiques peuvent :

- 🖱️ Afficher des **infobulles** au survol.
- 🔍 Permettre le **zoom** et le **déplacement** (pan).
- ✅ Être **sélectionnés** pour isoler des points.
- 🎬 S'**animer** dans le temps.

Et le meilleur ? Ces graphiques s'intègrent **nativement** dans ce site MkDocs. Vous allez le voir tout de suite.

!!! success "Objectif du chapitre"
    À la fin, vous saurez créer des graphiques interactifs impressionnants et les intégrer dans vos propres pages web ou notebooks.

---

## 📦 Installation et import

Dans Google Colab, Plotly est déjà installé. On utilise principalement le module `plotly.express` (alias `px`) qui est aussi simple que Seaborn.

```python
import plotly.express as px
import plotly.graph_objects as go  # Pour personnalisations avancées
import pandas as pd

# Jeux de données intégrés
df = px.data.gapminder()  # Données mondiales (PIB, espérance de vie, population)
df_tips = px.data.tips()  # Pourboires
```

---

## 🚀 1. Plotly Express : la simplicité avant tout

`plotly.express` fonctionne **exactement comme Seaborn** : vous passez un DataFrame, les noms des colonnes, et vous obtenez un graphique interactif.

### Nuage de points interactif
```python
fig = px.scatter(
    df.query("year == 2007"),  # Filtrer sur l'année 2007
    x="gdpPercap",
    y="lifeExp",
    size="pop",
    color="continent",
    hover_name="country",
    log_x=True,  # Échelle logarithmique pour le PIB
    size_max=60,
    title="Espérance de vie vs PIB par habitant (2007)"
)
fig.show()
```

!!! tip "Essayez !"
    Survolez les bulles : vous voyez le pays, le PIB, l'espérance de vie. Zoomez avec la molette. Cliquez sur une légende pour isoler un continent. **C'est ça, la magie Plotly.**

### Diagramme en barres
```python
fig = px.bar(
    df_tips,
    x="day",
    y="total_bill",
    color="sex",
    barmode="group",  # group, stack, overlay
    title="Addition moyenne par jour et par sexe"
)
fig.show()
```

### Histogramme
```python
fig = px.histogram(
    df_tips,
    x="total_bill",
    nbins=30,
    color="sex",
    marginal="rug",  # Ajoute des ticks sur l'axe
    title="Distribution des additions"
)
fig.show()
```

### Boxplot
```python
fig = px.box(
    df_tips,
    x="day",
    y="total_bill",
    color="smoker",
    title="Addition par jour et statut fumeur"
)
fig.show()
```

---

## 🎬 2. Animations : donnez vie à vos données

L'animation est l'une des fonctionnalités les plus impressionnantes de Plotly. Il suffit d'ajouter le paramètre `animation_frame`.

### Évolution de l'espérance de vie dans le temps (bulles animées)
```python
fig = px.scatter(
    df,  # Données de 1952 à 2007
    x="gdpPercap",
    y="lifeExp",
    size="pop",
    color="continent",
    hover_name="country",
    animation_frame="year",  # <-- La colonne qui pilote l'animation
    animation_group="country",
    log_x=True,
    size_max=55,
    range_y=[20, 90],
    range_x=[100, 100000],
    title="Évolution mondiale : PIB vs Espérance de vie (1952-2007)"
)
fig.show()
```

!!! info "Résultat"
    Vous verrez les bulles se déplacer au fil des années. Un **bouton Play** apparaît automatiquement. C'est l'effet "Hans Rosling" (célèbre conférencier TED).

### Graphique en lignes animé (évolution par continent)
```python
# Calculer la moyenne par continent et année
df_continent = df.groupby(['year', 'continent'], as_index=False)['lifeExp'].mean()

fig = px.line(
    df_continent,
    x="year",
    y="lifeExp",
    color="continent",
    line_group="continent",
    title="Évolution de l'espérance de vie moyenne par continent"
)
fig.show()
```

---

## 🛠️ 3. Personnalisation avancée avec `update_layout()`

Plotly Express retourne un objet `Figure` que vous pouvez modifier en profondeur.

```python
fig = px.scatter(
    df.query("year == 2007"),
    x="gdpPercap",
    y="lifeExp",
    size="pop",
    color="continent",
    hover_name="country",
    log_x=True,
    size_max=60
)

# Personnalisation du layout
fig.update_layout(
    title={
        'text': "Espérance de vie vs PIB (2007)",
        'font': {'size': 24, 'family': 'Arial', 'color': '#00e5ff'},
        'x': 0.5  # Centré
    },
    xaxis_title="PIB par habitant ($)",
    yaxis_title="Espérance de vie (années)",
    legend_title="Continent",
    template="plotly_dark",  # Thème sombre (ou "plotly_white", "seaborn", "ggplot2")
    font=dict(size=14),
    hovermode="closest"
)

# Personnaliser les traces (bulles)
fig.update_traces(
    marker=dict(line=dict(width=1, color='white')),
    selector=dict(mode='markers')
)

fig.show()
```

### Quelques templates prédéfinis :
- `plotly` (clair)
- `plotly_dark` (sombre)
- `ggplot2` (style R)
- `seaborn`
- `simple_white`
- `presentation` (pour slides)

---

## 📊 4. Dashboards simples avec `make_subplots`

Pour combiner plusieurs graphiques en un seul tableau de bord, on utilise `make_subplots`.

```python
from plotly.subplots import make_subplots

# Créer une grille 1 ligne, 2 colonnes
fig = make_subplots(
    rows=1, cols=2,
    subplot_titles=("Distribution", "Par jour"),
    specs=[[{"type": "histogram"}, {"type": "box"}]]
)

# Ajouter l'histogramme
hist = px.histogram(df_tips, x="total_bill", nbins=20)
for trace in hist.data:
    fig.add_trace(trace, row=1, col=1)

# Ajouter le boxplot
box = px.box(df_tips, x="day", y="total_bill")
for trace in box.data:
    fig.add_trace(trace, row=1, col=2)

# Ajuster la taille
fig.update_layout(height=500, width=1000, showlegend=False, title_text="Analyse des pourboires")
fig.show()
```

---

## 🌐 5. Intégrer un graphique Plotly dans MkDocs

C'est là que la magie opère pour **votre site**. Plotly génère du HTML que vous pouvez **inclure directement** dans une page Markdown.

### Méthode 1 : Sauvegarder en HTML et inclure
```python
fig.write_html("mon_graphique.html", include_plotlyjs='cdn')
```

Puis dans votre fichier `.md` :
```html
<iframe src="mon_graphique.html" width="100%" height="600px" frameborder="0"></iframe>
```

### Méthode 2 : Utiliser la syntaxe `plotly` dans MkDocs (si plugin activé)
Certains plugins MkDocs permettent d'écrire directement :
```markdown
```plotly
# Code Python Plotly ici
```
```

Mais la méthode `<iframe>` est universelle et fonctionne partout.

---

## 🧪 6. Exemple complet : Carte choroplèthe

Plotly excelle aussi dans les **cartes géographiques**.

```python
# Données sur les cas de COVID (exemple)
df_covid = pd.DataFrame({
    'country': ['France', 'Germany', 'Italy', 'Spain', 'USA'],
    'cases': [5000000, 4000000, 3500000, 3000000, 8000000],
    'iso_alpha': ['FRA', 'DEU', 'ITA', 'ESP', 'USA']
})

fig = px.choropleth(
    df_covid,
    locations="iso_alpha",
    color="cases",
    hover_name="country",
    color_continuous_scale=px.colors.sequential.Plasma,
    title="Cas de COVID par pays (exemple)"
)
fig.update_geos(projection_type="natural earth")
fig.show()
```

---

## ✅ Check-list du Visualiseur Plotly

- [ ] Importer `plotly.express as px`
- [ ] Créer un scatter, bar ou box interactif
- [ ] Ajouter une animation avec `animation_frame`
- [ ] Personnaliser le layout (`update_layout`)
- [ ] Changer le template (`plotly_dark`, etc.)
- [ ] Sauvegarder en HTML pour intégration

---

## 🔜 Prochaine étape

Vous avez maintenant toutes les cartes en main pour **explorer** et **raconter** vos données. La prochaine partie vous fera entrer dans le monde fascinant du **Machine Learning**, toujours avec Python et sans prise de tête.


---

*Fun fact : Plotly a été fondée par des ingénieurs canadiens passionnés de data viz. Leur bibliothèque est utilisée par la NASA, Google et... vous, maintenant.* 🚀