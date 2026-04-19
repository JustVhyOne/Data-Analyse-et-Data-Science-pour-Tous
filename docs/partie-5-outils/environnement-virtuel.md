# 📦 L'Environnement Virtuel

> *"Un environnement virtuel, c'est comme une cuisine séparée pour chaque recette. Comme ça, pas de mélange d'ingrédients."*

---

## 🎯 Le problème : quand tout se mélange

Imaginez : vous travaillez sur deux projets Python.

- **Projet A** : a besoin de `pandas==1.5.0`.
- **Projet B** : a besoin de `pandas==2.0.0` (car il utilise une nouvelle fonctionnalité).

Si vous installez les deux versions **globalement** sur votre ordinateur, la deuxième écrasera la première. Résultat : **le Projet A ne fonctionne plus**.

C'est le **cauchemar du data scientist**. Et la solution s'appelle : **l'environnement virtuel**.

---

## 🧪 Qu'est-ce qu'un environnement virtuel ?

Un environnement virtuel est un **dossier isolé** qui contient :

- Une copie de l'interpréteur Python.
- Les bibliothèques installées **spécifiquement pour ce projet**.

Quand vous activez un environnement virtuel, toutes les installations (`pip install`) vont dans ce dossier, sans affecter le reste de votre système.

!!! success "La règle d'or"
    **Un projet = un environnement virtuel**. Toujours.

---

## 🐍 Créer un environnement virtuel avec `venv`

`venv` est intégré à Python (depuis la version 3.3). Pas besoin d'installer quoi que ce soit.

### 1. Ouvrir un terminal

- **Windows** : PowerShell ou Invite de commandes.
- **Mac / Linux** : Terminal.

Placez-vous dans le dossier de votre projet :

```bash
cd mon_projet_data
```

### 2. Créer l'environnement

```bash
python -m venv env
```

- `env` est le nom du dossier qui contiendra l'environnement. Vous pouvez l'appeler comme vous voulez (`.venv`, `mon_env`, etc.). La convention est `env` ou `.venv`.

Un nouveau dossier `env/` apparaît.

### 3. Activer l'environnement

**Windows (PowerShell) :**
```powershell
.\env\Scripts\Activate.ps1
```

**Windows (Invite de commandes) :**
```cmd
env\Scripts\activate.bat
```

**Mac / Linux :**
```bash
source env/bin/activate
```

Si tout se passe bien, vous verrez `(env)` apparaître au début de votre ligne de commande :

```
(env) C:\mon_projet_data>
```

Cela signifie que vous êtes **dans l'environnement virtuel**.

### 4. Désactiver l'environnement

Quand vous avez fini de travailler :

```bash
deactivate
```

Le `(env)` disparaît. Vous êtes revenu à l'environnement global.

---

## 📦 Installer des bibliothèques dans l'environnement

Une fois l'environnement activé, `pip install` fonctionne normalement, mais **tout va dans le dossier `env/`**.

```bash
pip install pandas matplotlib seaborn scikit-learn
```

Pour vérifier ce qui est installé :

```bash
pip list
```

---

## 📋 Sauvegarder les dépendances : `requirements.txt`

Pour partager votre projet (ou le retrouver dans 6 mois), vous devez noter **exactement** quelles bibliothèques et versions sont utilisées.

```bash
pip freeze > requirements.txt
```

Cela crée un fichier `requirements.txt` qui ressemble à :

```
pandas==2.1.0
matplotlib==3.7.2
seaborn==0.12.2
scikit-learn==1.3.0
...
```

### Installer à partir d'un `requirements.txt`

Quand quelqu'un (ou vous sur une autre machine) clone votre projet :

```bash
# 1. Créer et activer un nouvel environnement
python -m venv env
source env/bin/activate  # ou .\env\Scripts\activate

# 2. Installer toutes les dépendances
pip install -r requirements.txt
```

Et voilà, l'environnement est **reproduit à l'identique**. ✨

---

## ☁️ Et sur Google Colab ?

Google Colab est un environnement **temporaire dans le cloud**. Il est déjà pré-installé avec les bibliothèques courantes (Pandas, NumPy, Matplotlib, etc.). Mais parfois, vous avez besoin d'une version spécifique ou d'une bibliothèque moins courante.

### Installer un package dans Colab

Dans une cellule :

```python
!pip install nom_du_package
```

Le `!` indique à Colab d'exécuter une commande shell.

### Créer un `requirements.txt` dans Colab

Si vous voulez noter les dépendances de votre notebook :

```python
!pip freeze > requirements.txt
```

Puis téléchargez le fichier :

```python
from google.colab import files
files.download('requirements.txt')
```

!!! warning "Attention"
    Colab réinstalle l'environnement à chaque nouvelle session. Vous devrez relancer les `!pip install` au début de chaque session. Pour automatiser, mettez-les dans la première cellule.

---

## 🧹 Bonnes pratiques

| ✅ À faire | ❌ À éviter |
|-----------|------------|
| Créer un environnement virtuel par projet | Installer tout en global |
| Ajouter `env/` au `.gitignore` (voir chapitre Git) | Commiter le dossier `env/` sur GitHub (il est énorme !) |
| Toujours avoir un `requirements.txt` à jour | Compter sur la mémoire pour se souvenir des versions |
| Utiliser des versions précises (`pandas==2.1.0`) | Utiliser `pandas` tout court (risque de casser plus tard) |

---

## 📁 Fichier `.gitignore`

Quand vous utiliserez Git, vous ne voulez **pas** envoyer le dossier `env/` (il peut faire plusieurs centaines de Mo). Créez un fichier `.gitignore` à la racine de votre projet avec :

```
env/
.venv/
__pycache__/
*.pyc
.DS_Store
```

---

## ✅ Check-list de l'environnement virtuose

- [ ] Créer un environnement avec `python -m venv env`
- [ ] L'activer (`activate` ou `source`)
- [ ] Installer les packages nécessaires
- [ ] Générer `requirements.txt` avec `pip freeze`
- [ ] Ajouter `env/` au `.gitignore`

---

## 🔜 Prochain chapitre

Votre environnement est maintenant propre et reproductible. Il est temps de le sauvegarder et de le partager avec le monde grâce à **Git et GitHub**.


---

*Fun fact : Le nom "venv" est la contraction de "Virtual Environment". Simple, efficace.* 🌿