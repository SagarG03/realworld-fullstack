# RealWorld full stack (Conduit)

A Medium-style blogging app ("Conduit") built to the [RealWorld](https://github.com/gothinkster/realworld) spec.

| Folder | What | Based on |
|---|---|---|
| [`api/`](api) | Node + Express + Prisma REST API (PostgreSQL) | [gothinkster/node-express-realworld-example-app](https://github.com/gothinkster/node-express-realworld-example-app) |
| [`frontend/`](frontend) | React + Redux + TypeScript web UI | [angelguzmaning/ts-redux-react-realworld-example-app](https://github.com/angelguzmaning/ts-redux-react-realworld-example-app) |

## Run locally

Prerequisites: Node.js 20 or 22 LTS and a PostgreSQL database.

### 1. API (http://localhost:3001/api)

```shell
cd api
npm install
```

Create `api/.env`:

```
DATABASE_URL=postgresql://USER:PASSWORD@localhost:5432/realworld?schema=public
JWT_SECRET=any-long-random-string
NODE_ENV=development
```

Make sure the database uses UTF-8 encoding, otherwise seeding fails.

```shell
npx prisma generate
npx prisma migrate deploy
npx prisma db seed      # optional sample data
PORT=3001 npx nx serve api
```

### 2. Frontend (http://localhost:4100)

```shell
cd frontend
npm install --legacy-peer-deps
PORT=4100 NODE_OPTIONS=--openssl-legacy-provider npm start
```

The frontend calls the API at `http://localhost:3001/api/` (set in `frontend/src/config/settings.ts`).

## Changes from the upstream projects

- `frontend/src/config/settings.ts`: points at the local API instead of the public demo API.
- `frontend/public/main.css`: the Conduit theme, copied from the RealWorld repo, because the old hosted stylesheet (`demo.productionready.io/main.css`) no longer exists.

## Known issues

- **Node 25 and newer:** `jsonwebtoken` depends on a package that uses `buffer.SlowBuffer`, which newer Node versions removed, so the API crashes. Use Node 22 LTS.
- **Seed script** prints errors but still exits successfully, so a partial seed can look like it worked.
