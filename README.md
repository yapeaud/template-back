# Backend

Template de backend Node.js en **TypeScript** basé sur **Express 5**.
Le code TypeScript est exécuté directement avec [`tsx`](https://github.com/privatenumber/tsx), sans étape de compilation, et rechargé automatiquement en développement avec `nodemon`.

## Stack

| Outil | Rôle |
| --- | --- |
| [Express 5](https://expressjs.com/) | Framework HTTP |
| [dotenv](https://github.com/motdotla/dotenv) | Chargement des variables d'environnement depuis `.env` |
| [TypeScript](https://www.typescriptlang.org/) | Typage statique |
| [tsx](https://github.com/privatenumber/tsx) | Exécution directe des fichiers `.ts` |
| [nodemon](https://nodemon.io/) | Redémarrage automatique en développement |

Le projet est configuré en **ES Modules** (`"type": "module"` dans `package.json`) : utilisez `import` / `export`, pas `require`.

## Prérequis

- [Node.js](https://nodejs.org/) (version LTS récente recommandée)
- npm (fourni avec Node.js)

## Installation

```bash
cd backend
npm install
```

## Configuration

Les variables d'environnement sont lues depuis un fichier `.env` à la racine de `backend/`.

1. Copiez le fichier d'exemple :

   ```bash
   cp .env.example .env
   ```

2. Renseignez les valeurs dans `.env`.

> `.env` contient des valeurs locales ou sensibles : il ne doit pas être commité. Documentez chaque nouvelle variable dans `.env.example` (sans sa valeur réelle).

Exemple de variable usuelle :

```env
PORT=3000
```

## Lancer le serveur

| Commande | Description |
| --- | --- |
| `npm run dev` | Démarre le serveur en mode développement avec rechargement automatique |
| `npm start` | Démarre le serveur une fois, sans rechargement |

## Structure du projet

```text
backend/
├── src/
│   └── server.ts      # Point d'entrée de l'application
├── .env               # Variables d'environnement locales (non commité)
├── .env.example       # Modèle des variables d'environnement
├── .gitignore
├── package.json
└── README.md
```

## Exemple de point de départ

Exemple minimal pour `src/server.ts` :

```ts
import "dotenv/config";
import express from "express";

const app = express();
const PORT = Number(process.env.PORT) || 3000;

app.use(express.json());

app.get("/health", (_req, res) => {
  res.json({ status: "ok" });
});

app.listen(PORT, () => {
  console.log(`Serveur démarré sur http://localhost:${PORT}`);
});
```

## Licence

ISC
