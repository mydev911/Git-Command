# PinDoc Step-1 Verification Commands

This document verifies that **Step-1 (Monorepo Scaffolding + Local Dev Infrastructure)** was created correctly.

---

# Verify Current Directory

| Command | Description |
|--------|-------------|
| `pwd` | Verify you are inside the pindoc project directory |
| `ls` | Show root files and folders created by Step-1 |

Expected root structure:

```
README.md
apps
docker-compose.yml
package.json
packages
pnpm-workspace.yaml
.gitignore
```

---

# Verify Hidden Files

| Command | Description |
|--------|-------------|
| `ls -la` | Show hidden files including `.gitignore` |

You should see:

```
.gitignore
README.md
apps
packages
docker-compose.yml
package.json
pnpm-workspace.yaml
```

---

# Verify Folder Structure

| Command | Description |
|--------|-------------|
| `find . -maxdepth 2 -type d` | Display directory structure |

Expected result:

```
.
./apps
./apps/api
./apps/web
./apps/worker
./packages
./packages/shared
```

---

# Verify Workspace Configuration

| Command | Description |
|--------|-------------|
| `cat pnpm-workspace.yaml` | Show pnpm workspace configuration |

Expected content example:

```
packages:
  - "apps/*"
  - "packages/*"
```

---

# Verify Root Package Configuration

| Command | Description |
|--------|-------------|
| `cat package.json` | Show root package configuration |

Verify scripts exist:

```
dev:db
db:down
db:logs
dev
```

---

# Verify Docker Configuration

| Command | Description |
|--------|-------------|
| `cat docker-compose.yml` | Show database container configuration |

Verify these values exist:

```
postgres:16
container_name: pindoc_db
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=pindoc
```

---

# Start Local Database

| Command | Description |
|--------|-------------|
| `docker compose up -d` | Start PostgreSQL container |
| `docker ps` | Confirm container is running |

Expected container name:

```
pindoc_db
```

---

# Check Database Logs

| Command | Description |
|--------|-------------|
| `docker logs pindoc_db --tail 30` | Check database startup logs |

Expected log example:

```
database system is ready to accept connections
```

---

# Verify PostgreSQL Connection

| Command | Description |
|--------|-------------|
| `docker exec -it pindoc_db psql -U postgres -d pindoc` | Connect to PostgreSQL database |

Inside PostgreSQL run:

```
SELECT version();
```

Exit PostgreSQL:

```
\q
```

---

# Stop Database

| Command | Description |
|--------|-------------|
| `docker compose down` | Stop PostgreSQL container |

---

# Step-1 Completion Checklist

Step-1 is successful if:

- Project folders exist
- Workspace configuration is correct
- Docker container starts successfully
- PostgreSQL connection works
- README verification commands work

After this verification, the project is ready for:

```
Step-2: Backend API scaffold
```
