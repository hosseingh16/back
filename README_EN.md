# Backend (Laravel API) – Local Development

This repository contains the **Laravel Backend API** for the project.

The backend runs inside **Docker on WSL2**, using:
- PHP 8.4 (default)
- Nginx + HTTPS (mkcert)
- MySQL
- phpMyAdmin

Frontend (Nuxt) is a **separate project**.

---

## Requirements

### Windows
- Windows 10/11
- WSL2 enabled
- Docker Desktop (WSL integration ON)
- mkcert installed on Windows

### Software
- Docker Desktop
- Git
- mkcert (Windows)

---

## Project Architecture

- Laravel API only (no frontend assets)
- DDD + Hexagonal Architecture
- Docker-based local environment
- HTTPS enabled locally via mkcert

---

## Folder Structure (Important)

```
.
├── app/
├── docker/
│   ├── nginx/
│   │   ├── nginx.conf
│   │   └── ssl/
│   ├── php/8.4/
│   │   ├── Dockerfile
│   │   └── php.ini
│   └── compose.yml
├── public/
├── storage/
└── .env
```

---

## First Time Setup (New Team Member)

### 1️⃣ Clone the repository

```bash
git clone <REPO_URL>
cd back
```

### 2️⃣ Add local domain

Edit **Windows hosts file**:

```
C:\Windows\System32\drivers\etc\hosts
```

Add:
```
127.0.0.1 back.local
```

---

### 3️⃣ Install mkcert (Windows)

If not installed:

```powershell
choco install mkcert
```

or download from:
https://github.com/FiloSottile/mkcert

Initialize mkcert:

```powershell
mkcert -install
```

Generate certificate:

```powershell
mkcert back.local
```

Copy generated files to:

```
docker/nginx/ssl/
```

Rename to:
- `back.local.pem`
- `back.local-key.pem`

---

### 4️⃣ Environment variables

Copy `.env.example`:

```bash
cp .env.example .env
```

Database defaults:

```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

---

### 5️⃣ Build & Run Docker

```bash
docker compose build
docker compose up -d
```

---

### 6️⃣ Install dependencies

```bash
docker compose exec php84 composer install
```

---

### 7️⃣ Run migrations

```bash
docker compose exec php84 php artisan migrate
```

---

## Access URLs

- Backend API: https://back.local
- phpMyAdmin: http://localhost:8080

---

## PHP Version

- Default: **PHP 8.4**
- PHP 8.5 support prepared (future)

---

## Common Commands

```bash
docker compose up -d
docker compose down
docker compose ps
docker compose logs -f
```

---

## Troubleshooting

### Permission denied (storage / cache)

```bash
docker compose exec php84 chown -R hossein:hossein storage bootstrap/cache
```

### HTTPS not secure

- Ensure mkcert is installed on **Windows**
- Certificate files exist in `docker/nginx/ssl`
- Restart containers

---

## Notes

- HTTPS is mandatory (API + Nuxt)
- Timezone: UTC
- OPCache enabled
- Xdebug enabled (DEV)

---

## Maintainer

Hossein  
Backend Architecture: DDD + Hexagonal  
