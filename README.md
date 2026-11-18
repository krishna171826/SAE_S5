# SAE S5 - Plateforme de Recommandation d'Activités Géolocalisées

Bienvenue sur le dépôt principal de notre projet **SAÉ BUT3**. **Spovo** est une application web permettant aux utilisateurs de rechercher des activités géolocalisées à faire entre amis (via OpenStreetMap et PostGIS), puis de les classer grâce à un algorithme de recommandation par score pondéré.

---

## 🛠️ Stack Technique

* **Frontend :** React (Vite) - Exécuté en local (hors Docker) pour un développement fluide.
* **Backend :** Node.js avec Express.js.
* **Moteur de Recommandation :** Python avec FastAPI.
* **Base de Données :** PostgreSQL avec l'extension spatiale **PostGIS**.
* **Orchestration :** Docker Compose (pour la base de données, le backend Express et le moteur Python).

---

## 📂 Architecture du Projet (Submodules)

### Qu'est-ce qu'un sous-module Git ?

Un sous-module permet d'inclure un dépôt Git indépendant à l'intérieur d'un autre. Dans notre projet, le dépôt principal pointe vers les sous-dossiers front et back. Il est donc crucial d'initialiser et de mettre à jour ces sous-modules correctement lors du clonage du projet.

Le dépôt principal utilise des **sous-modules Git** pour organiser le code source :

* `SAE_S5-Frontend/` : Code source de l'interface utilisateur (React).
* `SAE_S5-Backend/` : Code source du serveur Express et du sous-dossier `recommendation-engine/` (FastAPI).

---

## 🚀 Guide d'installation et de démarrage

Ce guide permet de lancer **Spovo** sur une machine locale, en suivant les étapes ci-dessous.

### 📋 Prérequis

Avant de commencer, assurez-vous d'avoir installé :

- **Git** — Pour récupérer le projet.
- **Docker Desktop** (avec Docker Compose) — Pour lancer la base de données, le backend et le moteur de recommandation.
- **Node.js 22.12 ou supérieur** et **npm** — Pour lancer l'interface React et importer les activités.
- **Une connexion Internet** — Pour le géocodage des adresses et les requêtes OpenStreetMap.

### 1. 📥 Cloner le projet

Ouvrez un terminal et exécutez :

```bash
git clone --recurse-submodules https://github.com/krishna171826/SAE_S5.git
cd SAE_S5
```

Cette commande récupère le dépôt principal ainsi que ses deux sous-modules : **Frontend** et **Backend**.

> **Si le projet a déjà été cloné sans ses sous-modules :**
>
> ```bash
> git submodule update --init --recursive
> ```

### 2. 🔧 Configurer l'environnement

Depuis le dossier principal `SAE_S5`, créez le fichier `.env` :

```bash
printf 'POSTGRES_PASSWORD=Choisir_Un_Mot_De_Passe\n' > .env
```

Ce fichier configure le mot de passe de PostgreSQL pour une utilisation locale.

> ⚠️ Ce mot de passe est uniquement destiné aux tests locaux.

### 3. 🐳 Démarrer les services Docker

Assurez-vous que Docker Desktop est démarré.

Toujours depuis le dossier principal `SAE_S5`, exécutez :

```bash
docker compose up -d --build
```

Cette commande construit et démarre automatiquement les trois services :

| Service | Technologie | Port |
|---|---|---|
| Base de données | PostgreSQL + PostGIS | `5433` |
| Backend API | Node.js + Express | `3000` |
| Moteur de recommandation | Python + FastAPI | `8000` |

Pour vérifier que les conteneurs sont démarrés :

```bash
docker compose ps
```

### 4. 🗄️ Initialiser la base de données

**Cette étape est indispensable lors de la première installation.**

Assurez-vous que les services Docker sont démarrés avant de continuer.

**1. Accéder au dossier Backend :**

Depuis le dossier principal `SAE_S5` :

```bash
cd SAE_S5-Backend
```

**2. Installer les dépendances :**

```bash
npm install
```

**3. Configurer le fichier `.env` du Backend :**

Copiez le fichier `.env.exemple` :

```bash
cp .env.exemple .env
```

Ouvrez ensuite le fichier `.env` et remplacez `VOTRE_MOT_DE_PASSE_ICI` par le mot de passe PostgreSQL défini à l'étape 2.

Par exemple, si vous avez conservé le mot de passe proposé :

```env
DATABASE_URL=postgres://sae:Choisir_Un_Mot_De_Passe@localhost:5433/sae_s5
```

**4. Exécuter les migrations :**

```bash
npm run migrate up
```

Cette commande crée automatiquement les tables nécessaires au fonctionnement de Spovo : utilisateurs, activités, catégories, recherches et recommandations.

> ✅ La structure de la base de données est maintenant initialisée.
>
> Les migrations déjà exécutées ne sont pas appliquées une deuxième fois.

**5. Importer les activités OpenStreetMap :**

```bash
npm run seed
```

Cette commande importe automatiquement dans PostgreSQL les **vraies activités OpenStreetMap d'Île-de-France**, préalablement extraites et enregistrées dans le fichier `data/activities-idf.json`.

L'importation peut prendre un certain temps lors de la première installation.

> ✅ Les activités sont maintenant enregistrées dans PostgreSQL.
>
> Aucun téléchargement supplémentaire ni extraction manuelle n'est nécessaire.
>
> Spovo pourra utiliser ces données même si les serveurs Overpass sont temporairement indisponibles.

**Source des données :** OpenStreetMap, extraction Île-de-France téléchargée depuis Geofabrik, sous licence ODbL. © [Contributeurs OpenStreetMap](https://www.openstreetmap.org/copyright).

### 5. 💻 Lancer le frontend React

Ouvrez un **deuxième terminal**, sans arrêter les services Docker.

Depuis le dossier principal `SAE_S5` :

```bash
cd SAE_S5-Frontend
npm ci
npm run dev
```

Le frontend est maintenant accessible à l'adresse :

### 🌐 http://localhost:5173

> Si le port `5173` est déjà utilisé, Vite peut proposer un autre port, comme `5174`. Utilisez alors l'adresse affichée dans le terminal.

---

## 🎯 Tester l'application

Une fois Spovo ouvert dans le navigateur :

1. **Prénom :** saisissez votre prénom.
2. **Adresse :** indiquez une adresse à partir de laquelle rechercher des activités.
3. **Catégories :** choisissez les activités souhaitées (Loisirs, Sport, Culture, Nature ou Restauration).
4. **Rayon :** sélectionnez une distance de recherche.
5. **Ambiance :** choisissez éventuellement Indoor ou Outdoor.
6. Cliquez sur **Voir mes recommandations**.

Spovo récupère les activités depuis OpenStreetMap, recherche les lieux situés dans le rayon choisi et les classe avec un algorithme de recommandation par score pondéré.

L'application dispose également d'activités réelles d'Île-de-France préchargées dans PostgreSQL. Elles sont utilisées lorsque les serveurs Overpass ne répondent pas.

> Les résultats dépendent des données disponibles dans OpenStreetMap. Si aucune activité n'est trouvée, essayez de sélectionner un rayon plus grand ou d'autres catégories.

---

## ✅ Vérifier le fonctionnement des API

Vous pouvez vérifier que le backend et le moteur Python fonctionnent grâce à ces commandes :

**Backend Express :**

```bash
curl http://localhost:3000/health
```

**Moteur Python FastAPI :**

```bash
curl http://localhost:8000/health
```

Les deux doivent retourner :

```json
{"status":"ok"}
```

---

## 🛠️ Commandes utiles

Toutes les commandes Docker suivantes sont à exécuter depuis le dossier principal `SAE_S5`.

**Afficher les conteneurs :**

```bash
docker compose ps
```

**Afficher les logs :**

```bash
docker compose logs -f
```

**Redémarrer les services :**

```bash
docker compose up -d --build
```

**Arrêter les services :**

```bash
docker compose down
```

> ⚠️ Ne pas utiliser `docker compose down -v` pour un arrêt normal : cette commande supprime également le volume contenant les données PostgreSQL.

---

## 🌿 Workflow de développement (Git / GitHub)

Le projet utilise GitHub pour organiser le travail en équipe.

1. **Issues :** répartition et suivi des tâches.
2. **Branches :** développement des fonctionnalités sur des branches dédiées.
3. **Commits :** enregistrement des modifications.
4. **Pull Requests :** relecture du code avant intégration.
5. **Merge :** fusion des modifications validées dans `main`.

Chaque sous-module dispose de son propre dépôt Git.

---
