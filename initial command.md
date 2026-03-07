# PinDoc Development Commands

## Environment Verification

| Command | Description |
|--------|-------------|
| `pwd` | Show current working directory |
| `ls` | List files and folders in current directory |
| `ls -la` | List all files including hidden files |
| `cd apps` | Navigate into apps folder |
| `cd ..` | Move back one directory |

---

# PNPM Workspace Commands

## Install Dependencies

| Command | Description |
|--------|-------------|
| `pnpm install` | Install all workspace dependencies |

---

# Docker Database Commands

## Start PostgreSQL Container

| Command | Description |
|--------|-------------|
| `docker compose up -d` | Start PostgreSQL container in background |
| `docker ps` | Show running Docker containers |
| `docker logs pindoc_db --tail 30` | View last 30 lines of database logs |
| `docker compose down` | Stop all containers defined in docker-compose |

---

# PostgreSQL Commands

## Verify Database Connection

| Command | Description |
|--------|-------------|
| `docker exec -it pindoc_db psql -U postgres -d pindoc` | Connect to Postgres inside Docker container |
| `SELECT version();` | Verify PostgreSQL server version |
| `\q` | Exit PostgreSQL shell |

---

# Project Structure Verification

## Verify Folder Structure

| Command | Description |
|--------|-------------|
| `find . -maxdepth 2 -type d` | Show directory structure |
| `cat pnpm-workspace.yaml` | Display workspace configuration |
| `cat package.json` | Show root project configuration |
| `cat docker-compose.yml` | Show docker configuration |
| `cat .gitignore` | Display ignored files configuration |
| `cat README.md` | Show project documentation |

---

# Git Configuration (Optional)

## Configure Git Identity

| Command | Description |
|--------|-------------|
| `git config --global user.name "Your Name"` | Set global git username |
| `git config --global user.email "you@example.com"` | Set global git email |
| `git config --list` | Verify git configuration |

---

# Git Repository Commands

## Initialize Repository

| Command | Description |
|--------|-------------|
| `git init` | Initialize Git repository |
| `git add .` | Stage all files for commit |
| `git commit -m "Initial project scaffolding"` | Create first commit |
| `git status` | Show git repository status |

---

# Useful Docker Debug Commands

| Command | Description |
|--------|-------------|
| `docker container ls` | Show running containers |
| `docker volume ls` | Show Docker volumes |
| `docker inspect pindoc_db` | Inspect container details |

---

# Cursor / Development Verification

| Command | Description |
|--------|-------------|
| `pwd` | Confirm working directory in Cursor terminal |
| `ls` | Verify project files created by Cursor |
| `pnpm install` | Install workspace dependencies |
| `docker compose up -d` | Start database container |

---

# Exit Commands

| Command | Description |
|--------|-------------|
| `\q` | Exit PostgreSQL CLI |
| `exit` | Exit terminal session |
