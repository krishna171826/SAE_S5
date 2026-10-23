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

---

### 2. Lancer l'infrastructure Backend & Base de données (avec Docker)
Les services lourds (PostgreSQL/PostGIS, Express et FastAPI) s'exécutent via Docker Compose. À la racine du projet, lancez :

```bash
docker compose up --build
```

Cela va démarrer :
* La base de données **PostgreSQL + PostGIS** sur le port .
* Le moteur de recommandation **FastAPI (Python)** sur le port .
* Le serveur **Express.js (Node.js)** sur le port .

---

### 3. Lancer le Frontend en local
Ouvrez un **nouveau terminal** sur votre machine (laissez Docker tourner dans le premier) pour lancer l'interface React :

```bash
cd sae-front
npm install
npm run dev
```

L'application web sera accessible localement sur : `http://localhost:5173`.

---

## 🩺 Vérification des services (Routes de santé)

Vous pouvez vérifier que chaque brique logicielle répond correctement :
* **Backend Express :** `http://localhost:3000/health`
* **Moteur Python (FastAPI) :** `http://localhost:8000/health`
* **Base de données :** Connectez-vous via DBeaver sur `localhost:5432` (utilisateurs/mdp définis dans le `compose.yaml`).

---

## 👥 Conventions de contribution
* Branches : `main` (production), `dev` (intégration), `feat/nom-de-fonctionnalite` (développement).
* Commits conventionnels conseillés : `feat:`, `fix:`, `docs:`, `refactor:`.
