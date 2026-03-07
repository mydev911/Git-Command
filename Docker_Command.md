# PinDoc Development Commands
Environment  
Windows 11 + WSL Ubuntu + Docker Desktop + Node + pnpm + PostgreSQL
---
## Check Current Folder
Command
```
pwd
```
Example Output
```
/home/swapo/projects/pindoc
```
---
## List Files and Folders
Command
```
ls
```

---

## List Files with Details

Command
```
ls -l
```

---

## Show Hidden Files

Command
```
ls -la
```

---

## Show Only Folders

Command
```
ls -d */
```

---

## Change Directory

Command
```
cd foldername
```

Example
```
cd ~/projects/pindoc
```

---

## Create Folder

Command
```
mkdir foldername
```

Create nested folder
```
mkdir -p ~/projects/pindoc
```

---

## Open Folder in Windows Explorer

Command
```
explorer.exe .
```

---

## Check Node Version

Command
```
node -v
```

---

## Check NPM Version

Command
```
npm -v
```

---

## Install PNPM

Command
```
sudo npm install -g pnpm
```

Verify
```
pnpm -v
```

---

## Check Docker Version

Command
```
docker --version
```

---

## Check Running Containers

Command
```
docker ps
```

---

## List All Containers

Command
```
docker ps -a
```

---

## List Docker Images

Command
```
docker images
```

---

## Run Docker Test

Command
```
docker run hello-world
```

Expected Output
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

---

## Start PostgreSQL Container

Command
```
docker run -d \
--name pindoc-postgres \
-e POSTGRES_USER=pindoc \
-e POSTGRES_PASSWORD=pindocpass \
-e POSTGRES_DB=pindocdb \
-p 5432:5432 \
postgres:16
```

Verify Container
```
docker ps
```

---

## Verify PostgreSQL Connection

Command
```
docker exec -it pindoc-postgres psql -U pindoc -d pindocdb
```

If successful you will see
```
pindocdb=#
```

Run Test Query
```
SELECT version();
```

Exit
```
\q
```

---

## Check Container Logs

Command
```
docker logs pindoc-postgres
```

---

## Stop Container

Command
```
docker stop pindoc-postgres
```

---

## Start Container

Command
```
docker start pindoc-postgres
```

---

## Remove Container

Command
```
docker rm pindoc-postgres
```

---

## Initialize Node Project

Command
```
pnpm init
```

---

## Install Prisma

Command
```
pnpm add prisma @prisma/client
```

---

## Initialize Prisma

Command
```
pnpm dlx prisma init
```

---

## Check Git Version

Command
```
git --version
```

---

## Initialize Git Repository

Command
```
git init
```

---

## Add Files to Git

Command
```
git add .
```

---

## Commit Changes

Command
```
git commit -m "initial commit"
```

---

## Check Git Status

Command
```
git status
```

---

## Open Project in VS Code

Command
```
code .
```

---

## Open Project in Cursor

Command
```
cursor .
```
