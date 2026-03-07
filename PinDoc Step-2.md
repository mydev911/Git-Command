# PinDoc Step-2 Commands (API + Prisma + Database)

This document lists all terminal commands used during **Step-2** of the PinDoc project setup.

Step-2 goal:

- Create Fastify API
- Connect Prisma ORM
- Connect PostgreSQL database
- Run database migration
- Start API server
- Verify `/health` endpoint

---

# Navigate to Project Root

| Command | Description |
|--------|-------------|
| `pwd` | Verify current working directory |
| `cd ~/projects/pindoc` | Navigate to the PinDoc project root |
| `ls` | Show project root files |

Expected structure:

```
README.md
apps
docker-compose.yml
package.json
packages
pnpm-workspace.yaml
```

---

# Verify Workspace Configuration

| Command | Description |
|--------|-------------|
| `cat pnpm-workspace.yaml` | Verify pnpm workspace configuration |

Expected content:

```
packages:
  - "apps/*"
  - "packages/*"
```

---

# Install Dependencies

| Command | Description |
|--------|-------------|
| `pnpm install` | Install all workspace dependencies |

This installs packages for:

```
apps/api
```

Including:

```
fastify
@fastify/cors
prisma
@prisma/client
typescript
ts-node-dev
```

---

# Start PostgreSQL Database

| Command | Description |
|--------|-------------|
| `docker compose up -d` | Start PostgreSQL container |
| `docker ps` | Verify container is running |
| `docker logs pindoc_db --tail 30` | Check database startup logs |

Expected container:

```
pindoc_db
```

---

# Navigate to API Service

| Command | Description |
|--------|-------------|
| `cd apps/api` | Enter API service directory |
| `ls` | Show API project files |

Expected files:

```
package.json
tsconfig.json
prisma/
src/
.env
```

---

# Verify Prisma Schema

| Command | Description |
|--------|-------------|
| `cat prisma/schema.prisma` | Display Prisma schema configuration |

Models included:

```
User
Document
```

---

# Run Database Migration

| Command | Description |
|--------|-------------|
| `pnpm prisma:migrate` | Run Prisma migration to create database tables |

This creates tables:

```
User
Document
```

inside PostgreSQL.

---

# Start API Server

| Command | Description |
|--------|-------------|
| `pnpm dev` | Start Fastify API server with ts-node-dev |

Expected log:

```
Database connection established
Pindoc API listening on http://localhost:3001
```

---

# Verify API Endpoint

Open a second terminal and run:

| Command | Description |
|--------|-------------|
| `curl http://localhost:3001/health` | Test API health endpoint |

Expected response:

```
{"status":"ok"}
```

---

# Stop API Server

| Command | Description |
|--------|-------------|
| `CTRL + C` | Stop the running API server |

---

# Stop Database

| Command | Description |
|--------|-------------|
| `docker compose down` | Stop PostgreSQL container |

---

# Optional Debug Commands

| Command | Description |
|--------|-------------|
| `lsof -i :3001` | Check which process is using port 3001 |
| `kill -9 <PID>` | Stop process using port 3001 |
| `docker container ls` | List running Docker containers |

---

# Step-2 Completion Checklist

Step-2 is successful if:

- Fastify API starts successfully
- Prisma connects to PostgreSQL
- Database migration completes
- `/health` endpoint returns status
- Server runs on port `3001`

Successful test result:

```
curl http://localhost:3001/health
{"status":"ok"}
```

After this step, the system is ready for:

```
Step-3: Cognito Authentication Integration
```
