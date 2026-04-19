# 🛑 Stop aux abonnements Excel, VBA et Tableau

> *"Pourquoi payer 120€ par an pour faire ce que 10 lignes de Python font mieux et gratuitement ?"*

---

## 💸 La douloureuse addition des logiciels propriétaires

Avant même de commencer à analyser une donnée, le professionnel "classique" sort sa carte bleue. Faisons un rapide calcul de ce que coûte une stack "standard" sur **3 ans**.

| Outil | Coût mensuel (estimé) | Coût sur 1 an | Coût sur 3 ans |
|-------|------------------------|---------------|----------------|
| Microsoft 365 (Excel, VBA) | 10 € | 120 € | **360 €** |
| Tableau Creator | 70 € | 840 € | **2 520 €** |
| Power BI Pro (si dépassement gratuit) | 10 € | 120 € | **360 €** |
| **TOTAL** | **~90 €/mois** | **~1 080 €** | **~3 240 €** |

!!! warning "Et ce n'est que la partie émergée de l'iceberg"
    - Ces prix sont **par utilisateur**. Dans une petite équipe de 3 personnes, la facture triple.
    - Vous êtes **dépendant** : l'éditeur augmente ses prix ? Vous subissez.
    - Vous voulez automatiser un rapport complexe ? Il faut souvent passer à la version "Enterprise" ($$$).

---

## 🐍 L'alternative Python : 0€, même puissance (voire plus)

Avec Python, le coût logiciel est strictement **zéro euro**. Le seul investissement est votre **temps d'apprentissage** – et ce site est là pour le réduire au minimum.

Voici la même stack, version open source :

| Besoin professionnel | Outil propriétaire | **Alternative Python (Gratuite)** |
|----------------------|-------------------|-----------------------------------|
| Tableur, manipulation de données | Excel | **Pandas** |
| Visualisations interactives | Tableau | **Plotly Express** |
| Dashboards | Power BI | **Streamlit** ou **Dash** |
| Automatisation de tâches répétitives | VBA (Macros) | **Python** (tout court) |

---

## 🔬 La preuve par l'exemple : Nettoyer un fichier de ventes

Imaginons une tâche typique : vous recevez un fichier `ventes_mars.csv` avec des colonnes mal nommées, des valeurs manquantes et des doublons.

=== "😰 Avec Excel / VBA"

    1. Ouvrir le fichier (temps de chargement si > 100 000 lignes).
    2. Supprimer les doublons manuellement via le ruban.
    3. Utiliser des filtres pour repérer les cellules vides.
    4. Créer une colonne "Prix Total" avec une formule (`=E2*F2`).
    5. Sauvegarder. Le fichier pèse maintenant 15 Mo.
    6. Si vous voulez refaire ça la semaine prochaine, recommencez les étapes 1 à 5 ou enregistrez une macro VBA (qui nécessite de coder dans un langage datant de 1993).

=== "🚀 Avec Python (Pandas)"

    ```python
    import pandas as pd

    # 1. Charger (même 1 million de lignes)
    df = pd.read_csv('ventes_mars.csv')

    # 2. Supprimer les doublons
    df.drop_duplicates(inplace=True)

    # 3. Supprimer les lignes vides
    df.dropna(inplace=True)

    # 4. Calculer le prix total
    df['Prix Total'] = df['Quantité'] * df['Prix Unitaire']

    # 5. Sauvegarder proprement
    df.to_csv('ventes_mars_clean.csv', index=False)

## 🔓 Les coûts cachés des logiciels propriétaires

Au-delà de l'abonnement, il y a des coûts indirects que l'on oublie souvent :

### 🕳️ Le **Vendor Lock-in** (Enfermement propriétaire)

- Vous avez passé 3 ans à développer des classeurs Excel avec des macros complexes.
- Un jour, votre entreprise décide de migrer sur Google Sheets ou LibreOffice.
- **Catastrophe** : Rien ne fonctionne. Vous devez tout refaire.

**Avec Python** : Le code Pandas tourne sur Windows, Mac, Linux, et même sur un serveur distant sans interface graphique. Vous êtes **libre**.

### 🐢 La **Performance en berne**

- Excel rame au-delà de 500 000 lignes. Tableau peut devenir lent avec des sources de données complexes.
- **Python (Pandas)** : Manipule des millions de lignes en mémoire vive sans sourciller.

### 🔒 La **Boîte Noire**

- Vous ne savez pas comment Tableau calcule tel ou tel agrégat. Si le résultat est bizarre, bonne chance pour debugger.
- **Avec Python** : Vous voyez chaque étape du calcul. C'est transparent.

---

## 🎯 Objectif de ce chapitre

À la fin de cette lecture, j'espère que vous ne verrez plus jamais le logo d'Excel de la même manière. Non pas qu'Excel soit un mauvais outil (c'est un excellent tableur), mais **ce n'est pas un outil de Data Analysis robuste et scalable**.

Dans le prochain chapitre, nous allons découvrir pourquoi le modèle économique de l'**Open Source** est plus **fiable** et **innovant** que celui des logiciels que vous payez.

---

  *Fun fact : Guido van Rossum, le créateur de Python, n'a jamais demandé un centime pour utiliser son langage. Et pourtant, il a changé le monde.* 🐍