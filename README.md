# Backend

Template de backend Node.js en **TypeScript** basé sur **Express 5** et **Prisma 7** (PostgreSQL).
Le code TypeScript est exécuté directement avec [`tsx`](https://github.com/privatenumber/tsx), sans étape de compilation, et rechargé automatiquement en développement avec `nodemon`.

## Stack

| Outil | Rôle |
| --- | --- |
| [Express 5](https://expressjs.com/) | Framework HTTP |
| [Prisma 7](https://www.prisma.io/) | ORM : schéma, migrations et client typé pour PostgreSQL |
| [dotenv](https://github.com/motdotla/dotenv) | Chargement des variables d'environnement depuis `.env` |
| [TypeScript](https://www.typescriptlang.org/) | Typage statique |
| [tsx](https://github.com/privatenumber/tsx) | Exécution directe des fichiers `.ts` |
| [nodemon](https://nodemon.io/) | Redémarrage automatique en développement |

Le projet est configuré en **ES Modules** (`"type": "module"` dans `package.json`) : utilisez `import` / `export`, pas `require`.

## Prérequis

- [Node.js](https://nodejs.org/) (version LTS récente recommandée)
- npm (fourni avec Node.js)
- Une base de données **PostgreSQL** accessible (locale, Docker ou hébergée)

## Installation

```bash
cd backend
npm install
npx prisma generate
```

`prisma generate` crée le client Prisma dans `src/generated/prisma/`. Ce dossier n'est pas commité : relancez la commande après chaque `npm install` et après chaque modification du schéma.

## Configuration

Les variables d'environnement sont lues depuis un fichier `.env` à la racine de `backend/`.

1. Copiez le fichier d'exemple :

   ```bash
   cp .env.example .env
   ```

2. Renseignez les valeurs dans `.env`.

> `.env` contient des valeurs locales ou sensibles : il ne doit pas être commité. Documentez chaque nouvelle variable dans `.env.example` (sans sa valeur réelle).

| Variable | Obligatoire | Description |
| --- | --- | --- |
| `DATABASE_URL` | Oui | Chaîne de connexion PostgreSQL utilisée par Prisma |
| `PORT` | Non | Port du serveur HTTP (`3000` par défaut) |

```env
DATABASE_URL="postgresql://utilisateur:motdepasse@localhost:5432/nom_de_la_base?schema=public"
PORT=3000
```

## Lancer le serveur

| Commande | Description |
| --- | --- |
| `npm run dev` | Démarre le serveur en mode développement avec rechargement automatique |
| `npm start` | Démarre le serveur une fois, sans rechargement |

Le serveur écoute sur `http://localhost:3000` (ou le port défini dans `PORT`). La route `GET /` renvoie `{ "status": "ok" }` et permet de vérifier qu'il tourne.

## Base de données (Prisma)

Le schéma de la base est décrit dans [`prisma/schema.prisma`](prisma/schema.prisma) et la configuration de la CLI dans [`prisma7.config.ts`](prisma7.config.ts) (chemin du schéma, dossier des migrations, lecture de `DATABASE_URL`).

| Commande | Description |
| --- | --- |
| `npx prisma generate` | Régénère le client Prisma après une modification du schéma |
| `npx prisma migrate dev --name <nom>` | Crée une migration à partir du schéma et l'applique à la base de développement |
| `npx prisma migrate deploy` | Applique les migrations existantes (production, CI) |
| `npx prisma studio` | Ouvre une interface web pour consulter et modifier les données |

Workflow habituel : modifier `schema.prisma` → `npx prisma migrate dev --name <nom>` → utiliser le client régénéré dans le code. Les migrations sont créées dans `prisma/migrations/` et doivent être commitées.

## Structure du projet

```text
backend/
├── prisma/
│   └── schema.prisma    # Schéma de la base de données
├── src/
│   ├── @types/          # Déclarations de types globales (ex. extension de Request)
│   ├── config/          # Configuration de l'application (variables d'env., client Prisma…)
│   ├── controllers/     # Traitement des requêtes HTTP et envoi des réponses
│   ├── generated/       # Client Prisma généré (non commité)
│   ├── helpers/         # Fonctions utilitaires réutilisables
│   ├── middlewares/     # Middlewares Express (auth, gestion d'erreurs…)
│   ├── models/          # Types et modèles métier
│   ├── routes/          # Définition des routes et association aux contrôleurs
│   ├── services/        # Logique métier et accès aux données via Prisma
│   ├── validators/      # Validation des données entrantes
│   └── server.ts        # Point d'entrée de l'application
├── .env                 # Variables d'environnement locales (non commité)
├── .env.example         # Modèle des variables d'environnement
├── .gitignore
├── package.json
├── prisma7.config.ts    # Configuration de la CLI Prisma
└── README.md
```

Les dossiers encore vides contiennent un fichier `.gitkeep` pour être versionnés ; supprimez-le dès que le dossier contient du code.

## Licence

ISC
