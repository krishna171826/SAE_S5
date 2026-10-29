# SAE S5 - Plateforme de Recommandation d'Activités Géolocalisées

Bienvenue sur le dépôt principal de notre projet **SAÉ BUT3**. Cette application web innovante permet aux utilisateurs de rechercher des activités géolocalisées (via OpenStreetMap et PostGIS) et compare les performances de différents algorithmes de recommandation.

---

## 🛠️ Stack Technique

* **Frontend :** React (Vite) - Exécuté en local (hors Docker) pour un développement fluide.
* **Backend :** Node.js avec Express.js.
* **Moteur de Recommandation :** Python avec FastAPI.
* **Base de Données :** PostgreSQL avec l'extension spatiale **PostGIS**.
* **Orchestration :** Docker Compose (pour la base de données, le backend Express et le moteur Python).

---

## 📂 Architecture du Projet (Submodules)

# Qu'est-ce qu'un sous-module Git ?
Un sous-module permet d'inclure un dépôt Git indépendant à l'intérieur d'un autre. Dans notre projet, le dépôt principal pointe vers les sous-dossiers front et back. Il est donc crucial d'initialiser et de mettre à jour ces sous-modules correctement lors du clonage du projet.


Le dépôt principal utilise des **sous-modules Git** pour organiser le code source :
* `sae-front/` : Code source de l'interface utilisateur (React).
* `sae-back/` : Code source du serveur Express et du sous-dossier `recommendation-engine/` (FastAPI).

---

## 🚀 Guide d'installation et de démarrage

### 1. Cloner le projet (avec les submodules)
Si vous clonez le projet pour la toute première fois, utilisez l'option `--recursive` pour récupérer automatiquement les sous-dossiers :

```bash
git clone --recursive https://github.com/votre-compte/SAE_S5.git
cd SAE_S5
```

> *Si le dépôt est déjà cloné sans les sous-modules, initialisez-les avec :*
> ```bash
> git submodule update --init --recursive
> ```
>
---

## 🌿 Workflow de Développement & Collaboration (Git / GitHub)

Pour travailler efficacement en équipe et éviter les conflits, nous adoptons le processus suivant :

1. **Gestion des tâches (Issues) :** Pour toute nouvelle fonctionnalité ou correction, référez-vous à l'onglet **Issues** de GitHub pour identifier votre tâche.
2. **Création d'une branche dédiée :** Ne codez jamais directement sur la branche principale (`main`). Créez une nouvelle branche pour votre fonctionnalité :
   ```bash
   git checkout -b feature/nom-de-la-fonctionnalite
   ```
3. **Développement et commit :** Effectuez vos modifications, testez-les localement, puis poussez votre branche sur GitHub :
   ```bash
   git add .
   git commit -m "feat: ajout de la fonctionnalité X"
   git push origin feature/nom-de-la-fonctionnalite
   ```
4. **Pull Request (PR) :** Une fois le travail terminé, ouvrez une **Pull Request (PR)** sur GitHub.
5. **Validation et Fusion :** **Informez le chef d'équipe** que votre PR est prête. Il procédera à la relecture et effectuera le **`git merge`** sur la branche principale.

---

---

### 2. Lancer l'infrastructure Backend & Base de données (avec Docker)
Ce projet regroupe une base de données **PostgreSQL** avec l'extension **PostGIS** (indispensable pour la gestion des coordonnées géographiques et spatiales), un **backend Express**, et un **système de recommandation**. L'ensemble est orchestré via **Docker Compose** pour un lancement unifié en une seule commande.

```bash
docker compose up --build
```

Cela va démarrer :
* La base de données **PostgreSQL + PostGIS** sur le port .
* Le moteur de recommandation **FastAPI (Python)** sur le port .
* Le serveur **Express.js (Node.js)** sur le port .

  ## Commandes utiles

- **Voir les logs en temps réel :**
  ```bash
  docker compose logs -f
  ```

- **Arrêter proprement tous les services :**
  ```bash
  docker compose down
  ```

- **Arrêter et supprimer les volumes (attention, cela efface les données de la base) :**
  ```bash
  docker compose down -v
  ```

---

### 3. Lancer le Frontend en local
Ouvrez un **nouveau terminal** sur votre machine (laissez Docker tourner dans le premier) pour lancer l'interface React :

```bash
cd sae-front
npm install
npm run dev
```
