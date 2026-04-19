# 🐙 Git & GitHub Gratuit

> *"Git, c'est comme une machine à remonter le temps pour votre code. GitHub, c'est la bibliothèque mondiale où vous exposez vos chefs-d'œuvre."*

---

## 🎯 Pourquoi Git et GitHub changent la donne

Vous avez passé des heures à écrire du code, à peaufiner vos analyses. Et là, c'est le drame :

- Vous supprimez accidentellement un fichier important.
- Vous voulez revenir à la version d'il y a 3 jours, mais vous avez écrasé le fichier.
- Vous devez collaborer avec quelqu'un et vous vous échangez des ZIP par email nommés `projet_final_v2_vraiment_final_OK.zip`.

**Git** résout **tous** ces problèmes. C'est un **système de contrôle de version** qui enregistre l'historique de vos modifications.

**GitHub** est une plateforme en ligne qui héberge vos dépôts Git et facilite la collaboration. C'est aussi votre **portfolio de data scientist**.

Et le meilleur ? **C'est gratuit**.

---

## 🤔 Git vs GitHub : ne pas confondre

| Git | GitHub |
|-----|--------|
| Un **logiciel** installé sur votre ordinateur | Un **site web** (github.com) |
| Gère l'historique **localement** | Héberge votre code **en ligne** |
| Fonctionne sans internet | Permet le partage et la collaboration |
| Créé par Linus Torvalds en 2005 | Créé en 2008, appartient à Microsoft |

---

## 📦 1. Installer Git

### Windows
Téléchargez l'installateur sur [git-scm.com](https://git-scm.com/download/win). Choisissez les options par défaut.

### Mac
```bash
brew install git
```
Ou téléchargez depuis le site.

### Linux (Ubuntu/Debian)
```bash
sudo apt install git
```

### Vérifier l'installation
```bash
git --version
# Doit afficher quelque chose comme : git version 2.40.0
```

---

## ⚙️ 2. Configuration initiale (à faire une seule fois)

Ouvrez un terminal et configurez votre identité :

```bash
git config --global user.name "Justicier VIHYA"
git config --global user.email "votre.email@exemple.com"
```

Ces informations seront attachées à chaque modification que vous faites.

---

## 🗂️ 3. Créer un dépôt Git local

Placez-vous dans le dossier de votre projet MkDocs :

```bash
cd mon_site_data_science
```

Initialisez le dépôt Git :

```bash
git init
```

Un dossier caché `.git/` est créé. Il contient **tout l'historique** du projet.

!!! warning "État des lieux"
    Pour l'instant, Git ne suit aucun fichier. Vous devez lui dire lesquels sauvegarder.

---

## 📸 4. Le cycle de vie : add → commit

Git fonctionne en trois étapes :

1. **Modifier** des fichiers dans votre dossier de travail.
2. **Ajouter** (`add`) les modifications à la "zone de staging" (la liste des changements qui seront sauvegardés).
3. **Valider** (`commit`) pour créer un point de sauvegarde dans l'historique.

### Premier commit

```bash
# Ajouter tous les fichiers du dossier actuel (.)
git add .

# Créer un commit avec un message descriptif
git commit -m "Première version du site Data Science"
```

Félicitations ! Vous venez de créer votre premier **commit**. Votre code est maintenant versionné localement.

---

## 📜 5. Consulter l'historique

```bash
git log --oneline
```

Affiche la liste des commits avec leur identifiant et leur message :

```
a1b2c3d Première version du site Data Science
```

---

## 🌐 6. Créer un dépôt sur GitHub

1. Allez sur [github.com](https://github.com) et connectez-vous (créez un compte gratuit si nécessaire).
2. Cliquez sur le **+** en haut à droite → **New repository**.
3. Donnez un nom (ex: `mon-site-data-science`).
4. Laissez le dépôt **Public** (gratuit) ou **Private** (payant, mais des offres gratuites existent pour les étudiants).
5. **Ne cochez pas** "Initialize this repository with a README" (car vous avez déjà un dépôt local).
6. Cliquez sur **Create repository**.

---

## 🔗 7. Lier le dépôt local à GitHub

GitHub vous affiche des instructions. Copiez les commandes de la section **"…or push an existing repository from the command line"** :

```bash
git remote add origin https://github.com/votre-username/mon-site-data-science.git
git branch -M main
git push -u origin main
```

- `remote add origin` : indique à Git l'adresse de votre dépôt en ligne (surnommé `origin`).
- `branch -M main` : renomme la branche principale en `main` (standard actuel).
- `push -u origin main` : envoie vos commits locaux vers GitHub.

Entrez votre nom d'utilisateur et votre mot de passe (ou token) GitHub si demandé.

Rafraîchissez la page GitHub : **votre code est en ligne !** 🎉

---

## 🔄 8. Le cycle quotidien : add, commit, push

Chaque fois que vous modifiez votre site et voulez sauvegarder :

```bash
# 1. Voir ce qui a changé
git status

# 2. Ajouter les modifications
git add .

# 3. Créer un commit avec un message clair
git commit -m "Ajout du chapitre sur Git et GitHub"

# 4. Envoyer sur GitHub
git push
```

---

## 📂 9. Le fichier `.gitignore`

Certains fichiers/dossiers **ne doivent jamais** être envoyés sur GitHub :

- Le dossier `env/` (environnement virtuel) : trop lourd.
- Les fichiers de configuration personnels.
- Les données sensibles (mots de passe).

Créez un fichier nommé `.gitignore` à la racine de votre projet :

```
# Environnement virtuel
env/
.venv/

# Cache Python
__pycache__/
*.pyc

# Fichiers système
.DS_Store
Thumbs.db

# Fichiers de build MkDocs
site/
```

!!! tip "Astuce"
    Ajoutez ce fichier **avant** votre premier `git add`. Sinon, Git aura déjà commencé à suivre les dossiers indésirables.

---

## 🚀 10. Déployer votre site MkDocs avec GitHub Pages

GitHub peut héberger **gratuitement** des sites statiques comme celui que vous avez créé avec MkDocs. C'est le service **GitHub Pages**.

### Étape 1 : Configurer MkDocs pour GitHub Pages

Dans votre `mkdocs.yml`, assurez-vous d'avoir :

```yaml
site_url: https://votre-username.github.io/mon-site-data-science/
```

### Étape 2 : Builder le site

En local, générez le dossier `site/` contenant le site final :

```bash
mkdocs build
```

### Étape 3 : Déployer automatiquement avec `gh-deploy`

MkDocs a une commande magique :

```bash
mkdocs gh-deploy
```

Cette commande :
- Build le site.
- Crée une branche spéciale `gh-pages`.
- Pousse le contenu du dossier `site/` sur cette branche.

Après quelques minutes, votre site sera accessible à l'adresse :
```
https://votre-username.github.io/mon-site-data-science/
```

!!! success "Votre site est en ligne !"
    Partagez ce lien fièrement. Vous avez maintenant un site web professionnel, hébergé gratuitement, avec votre nom de domaine GitHub.

---

## 🤝 11. Collaborer (les bases)

Si quelqu'un veut contribuer à votre projet :

1. Il **fork** votre dépôt (copie sur son compte).
2. Il **clone** son fork en local.
3. Il fait des modifications, commit, push.
4. Il ouvre une **Pull Request** (demande d'intégration) sur GitHub.
5. Vous révisez et acceptez (ou non).

C'est ainsi que fonctionnent les plus grands projets open source (Pandas, Scikit-learn, Linux...).

---

## ✅ Check-list du Git Master

- [ ] Installer Git et configurer `user.name` / `user.email`
- [ ] `git init` dans un projet
- [ ] Créer un `.gitignore`
- [ ] `git add .` + `git commit -m "message"`
- [ ] Créer un dépôt sur GitHub
- [ ] Lier avec `git remote add origin ...`
- [ ] `git push -u origin main`
- [ ] Déployer avec `mkdocs gh-deploy`

---

## 🎉 Félicitations finales !

Vous avez terminé l'intégralité de ce parcours **Data Analyse et Data Science pour tous avec Python**.

Vous savez maintenant :

- 💸 Pourquoi les outils open source et gratuits sont supérieurs.
- 🐍 Les bases de Python pour la Data.
- 🐼 Manipuler des données avec Pandas.
- 📊 Visualiser avec Seaborn et Plotly.
- 🧠 Entraîner un modèle de Machine Learning.
- 🛠 Gérer votre environnement et versionner votre code comme un pro.

Le plus important : **vous avez construit quelque chose de concret**. Ce site en est la preuve.

---

## 🧭 Et maintenant ?

- 📢 Partagez votre site sur LinkedIn, Twitter/X.
- 🔁 Continuez à l'enrichir avec de nouveaux articles.
- 🧪 Explorez d'autres bibliothèques : **Streamlit** pour des dashboards, **PyTorch** pour le Deep Learning.
- 🤝 Contribuez à des projets open source sur GitHub.

Le monde de la Data est immense, mais vous avez maintenant les clés pour l'explorer.

**Bravo, et à bientôt pour de nouvelles aventures data !** 🚀🐍

---

*Fun fact : Le logo de GitHub est un "Octocat", un chat à cinq tentacules. Il est devenu l'un des mascottes les plus aimées du monde tech.* 🐙😺