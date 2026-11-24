# MyAppReact - Dockerisation d'une application MERN

## 1. Présentation du projet

Ce projet consiste à dockeriser une application composée de trois services :

- **Client :** application React.
- **Server :** API développée avec Express.js.
- **MongoDB :** base de données NoSQL.

L'objectif est de construire les images Docker et de lancer les différents services avec Docker Compose.

## 2. Structure du projet

```text
MyAppReact/
│
├── Client/
│   ├── Dockerfile
│   ├── package.json
│   └── ...
│
├── Server/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│       └── index.mjs
│
├── docker-compose.yml
└── README.md
```

## 3. Création des Dockerfiles

### 3.1 Dockerfile du serveur

Le fichier Dockerfile du serveur utilise l'image `node:lts-alpine`.

Les principales instructions sont :

- Définir le répertoire de travail `/app`.
- Copier les fichiers `package.json` et `package-lock.json`.
- Installer les dépendances avec `npm install`.
- Copier le reste des fichiers du serveur.
- Exposer le port `9000`.
- Démarrer l'application avec `npm start`.

L'image créée est nommée `mern-server`.

### 3.2 Dockerfile du client

Le Dockerfile du client utilise également `node:lts-alpine`.

Les étapes réalisées sont :

- Définir le répertoire de travail `/app`.
- Copier les fichiers `package*.json`.
- Installer les dépendances avec `npm install`.
- Copier les fichiers du client.
- Construire l'application React avec `npm run build`.
- Installer le serveur HTTP `serve`.
- Exposer le port `3000`.
- Servir les fichiers de production du dossier `dist`.

L'image créée est nommée `mern-client`.

## 4. Création du fichier Docker Compose

Le fichier `docker-compose.yml` définit trois services :

### MongoDB
- Utilise l'image officielle `mongo`.
- Nom du conteneur : `mongodb`.
- Connecté au réseau `mern-network`.

### Server
- Construit à partir du dossier `./Server`.
- Nom du conteneur : `server`.
- Expose le port `9000`.
- Dépend du service MongoDB.
- Utilise la variable d'environnement `MONGO_URI`.

### Client
- Construit à partir du dossier `./Client`.
- Nom du conteneur : `client`.
- Expose le port `3000`.
- Dépend du service Server.

### Réseau

Un réseau Docker nommé `mern-network` est créé avec le driver `bridge` afin de permettre la communication entre les conteneurs.

## 5. Construction et lancement

Pour construire les images individuellement :

```bash
docker build -t mern-server ./Server
docker build -t mern-client ./Client
```

Pour lancer tous les services avec Docker Compose :

```bash
docker compose up --build -d
```

## 6. Vérification des conteneurs

Pour vérifier l'état des conteneurs :

```bash
docker compose ps
```

Pour consulter les logs :

```bash
docker compose logs
```

Pour consulter les logs du serveur :

```bash
docker compose logs server
```

## 7. Accès à l'application

Les services sont accessibles aux adresses suivantes :

| Service | Adresse |
|---|---|
| React Client | http://localhost:3000 |
| Express Server | http://localhost:9000 |
| API Health | http://localhost:9000/api/health |

Le test de l'API Health retourne :

```json
{
  "message": "API is running"
}
```

## 8. Arrêt des services

Pour arrêter et supprimer les conteneurs et le réseau créé par Docker Compose :

```bash
docker compose down
```

## 9. Conclusion

La dockerisation de l'application a été réalisée avec succès.

Les trois services (Client, Server et MongoDB) sont exécutés dans des conteneurs Docker et communiquent à travers le réseau `mern-network`.

Docker Compose permet de construire, lancer et gérer l'ensemble de l'application avec une seule commande.
