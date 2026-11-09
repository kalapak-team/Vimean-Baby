<div align="center">

<img src="user-frontend/public/logo.svg" alt="Vimean Baby" height="80" />

# Vimean Baby

**Mother & Baby E-Commerce Platform for Cambodia**

A production-ready, full-stack monorepo serving a bilingual (Khmer / English) storefront, an admin dashboard, and a REST API backend — built for mobile-first shoppers across Cambodia.

[![Java](https://img.shields.io/badge/Java-21-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://openjdk.org/projects/jdk/21/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.4-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)](https://spring.io/projects/spring-boot)
[![Nuxt.js](https://img.shields.io/badge/Nuxt-3-00DC82?style=for-the-badge&logo=nuxt.js&logoColor=white)](https://nuxt.com)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)](https://tailwindcss.com)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Project Structure](#-project-structure)
- [Tech Stack](#-tech-stack)
- [Prerequisites](#-prerequisites)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [API Reference](#-api-reference)
- [Design System](#-design-system)
- [Deployment](#-deployment)
- [Development Roadmap](#-development-roadmap)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🛍️ Overview

**Vimean Baby** is an e-commerce platform specializing in mother and baby products — diapers, wipes, bath products, feeding items, baby clothing, and maternity items — designed for the Cambodian market.

**Key highlights:**

- 🇰🇭 **Bilingual** — Khmer (default) and English with Noto Sans Khmer / Kantumruy Pro fonts
- 📱 **Mobile-first** — optimized for the smartphone-dominant Cambodian market
- 💵 **Local payments** — Cash on Delivery & Bank Transfer (ABA PayWay / KHQR roadmapped)
- 🔒 **Secure** — JWT authentication, role-based access control, BCrypt passwords
- 🏗️ **Production-ready** — Docker, Railway (backend), Vercel (frontends), Neon (DB), Cloudinary (media)

---

## 🏗️ Architecture

```
┌─────────────────────────────┐     ┌──────────────────────────────┐
│     user-frontend            │     │     admin-frontend            │
│   Nuxt 3 SSR  :3000          │     │   Nuxt 3 SPA  :3001           │
│   Khmer / English storefront │     │   Internal dashboard          │
└──────────────┬──────────────┘     └──────────────┬───────────────┘
               │  REST /api                        │  REST /api
               └────────────────┬──────────────────┘
                                 ▼
                   ┌─────────────────────────┐
                   │   backend               │
                   │   Spring Boot  :8080    │
                   │   JWT · Swagger UI      │
                   └────────────┬────────────┘
                                ▼
              ┌─────────────────────────────────┐
              │  PostgreSQL 16                   │
              │  local: Docker  vimeanbaby_db    │
              │  prod:  Neon (serverless)        │
              └─────────────────────────────────┘
                                │
                   ┌────────────▼────────────┐
                   │  Cloudinary             │
                   │  Product image storage  │
                   └─────────────────────────┘
```

---

## 📁 Project Structure

```
VimeanBaby/
├── backend/                        # Spring Boot REST API
│   ├── src/main/java/com/vimeanbaby/
│   │   ├── config/                 # Security, CORS, Cloudinary, OpenAPI
│   │   ├── security/               # JWT filter & service
│   │   ├── common/                 # ApiResponse, PageResponse, BaseEntity
│   │   ├── exception/              # GlobalExceptionHandler, custom exceptions
│   │   ├── user/                   # Users, roles, addresses
│   │   ├── catalog/                # Categories, brands, products, variants, images
│   │   ├── cart/                   # Cart & cart items
│   │   ├── order/                  # Orders, order items, status history
│   │   ├── payment/                # COD, bank transfer (ABA/KHQR roadmapped)
│   │   ├── shipping/               # Province-based shipping zones
│   │   ├── media/                  # Cloudinary upload/delete
│   │   └── dashboard/              # Admin analytics summary
│   ├── src/main/resources/db/migration/   # Flyway SQL migrations
│   ├── Dockerfile                  # Production Docker image
│   ├── railway.toml                # Railway deployment config
│   └── .env.example
│
├── user-frontend/                  # Customer storefront (Nuxt 3, SSR)
│   ├── pages/                      # Home, products, categories, cart, checkout, account
│   ├── components/                 # Layout, product, cart, checkout, UI primitives
│   ├── stores/                     # Pinia: auth, cart
│   ├── composables/                # useApi, useAuth, useCart, useCurrency
│   ├── i18n/locales/               # km.json, en.json
│   └── .env.example
│
├── admin-frontend/                 # Admin panel (Nuxt 3, SPA)
│   ├── pages/                      # Dashboard, products, orders, customers, shipping
│   ├── components/                 # Sidebar, DataTable, ProductForm, OrderStepper
│   ├── stores/                     # Pinia: auth
│   └── .env.example
│
├── docs/                           # Additional documentation
│   ├── MASTER_PROMPT.md
│   ├── api-endpoints.md
│   ├── database-diagram.md
│   ├── design-palette.md
│   ├── environments.md
│   └── railway-backend.md
│
└── docker-compose.yml              # Local dev: PostgreSQL + backend
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Backend API** | Java 21 · Spring Boot 3.4 · Spring Security (JWT) · Spring Data JPA · Validation · Lombok · MapStruct |
| **Database** | PostgreSQL 16 · Flyway (schema migrations) |
| **API Docs** | springdoc-openapi (Swagger UI at `/swagger-ui.html`) |
| **Customer Frontend** | Nuxt 3 (SSR) · Vue 3 · TypeScript · Tailwind CSS · Pinia · @nuxtjs/i18n · VueUse |
| **Admin Frontend** | Nuxt 3 (SPA) · Vue 3 · TypeScript · Tailwind CSS · Pinia |
| **Media Storage** | Cloudinary (product images; same cloud for local & production) |
| **Local Dev DB** | Docker Compose → PostgreSQL 16 (`vimeanbaby_db`) |
| **Production DB** | Neon (serverless PostgreSQL) |
| **Deployment** | Railway (backend via Docker) · Vercel (both frontends) |

---

## ✅ Prerequisites

| Requirement | Version | Notes |
|---|---|---|
| Java (JDK) | 21+ | OpenJDK or Eclipse Temurin |
| Maven | 3.9+ | Or use `./mvnw` wrapper in `backend/` |
| Node.js | 20+ | LTS recommended |
| Docker Desktop | Latest | Required for local PostgreSQL |
| PostgreSQL client | Any | pgAdmin or `psql` for inspection |

> **Cloudinary account** is required for image uploads. Set the three `CLOUDINARY_*` env vars in `backend/.env`.

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/<your-org>/VimeanBaby.git
cd VimeanBaby
```

### 2. Start the Local Database

```bash
docker compose up -d postgres
```

This creates the `vimeanbaby_db` database on `localhost:5432`.

### 3. Backend Setup

```bash
cd backend
cp .env.example .env
# Fill in DB credentials and Cloudinary keys in .env
./mvnw spring-boot:run          # Linux / macOS
.\mvnw.cmd spring-boot:run      # Windows
```

Flyway runs all migrations automatically on startup.

| Endpoint | URL |
|---|---|
| API root | http://localhost:8080 |
| Swagger UI | http://localhost:8080/swagger-ui.html |
| Health check | http://localhost:8080/actuator/health |

### 4. Customer Storefront

```bash
cd user-frontend
cp .env.example .env
npm install
npm run dev
```

✅ Open [http://localhost:3000](http://localhost:3000)

### 5. Admin Panel

```bash
cd admin-frontend
cp .env.example .env
npm install
npm run dev
```

✅ Open [http://localhost:3001](http://localhost:3001)

### Full Stack (Docker)

Spin up PostgreSQL **and** the Spring Boot backend together:

```bash
docker compose up --build
```

---

## 🔐 Environment Variables

> ⚠️ **Never commit real secrets.** Use `.env.example` files as templates only. All secret files are listed in `.gitignore`.

### `user-frontend/.env` & `admin-frontend/.env`

```env
NUXT_PUBLIC_API_BASE=http://localhost:8080/api
```

### `backend/.env` (local development)

```env
# Database
DB_URL=jdbc:postgresql://localhost:5432/vimeanbaby_db
DB_USERNAME=postgres
DB_PASSWORD=postgres123

# Security
JWT_SECRET=change-me-to-a-long-random-secret-at-least-256-bits-long

# Cloudinary (get from cloudinary.com → Dashboard)
CLOUDINARY_CLOUD_NAME=vimeanbaby
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# CORS
CORS_ALLOWED_ORIGINS=http://localhost:3000,http://localhost:3001

# Default admin seed (created on first boot)
ADMIN_EMAIL=admin@vimeanbaby.com
ADMIN_PASSWORD=change_this_password
ADMIN_FULL_NAME=Vimean Baby Admin
```

### `backend/.env.production` (or Railway Variables)

```env
SPRING_PROFILES_ACTIVE=prod

# Neon Database (JDBC format)
DB_URL=jdbc:postgresql://<host>/neondb?sslmode=require
DB_USERNAME=neondb_owner
DB_PASSWORD=<neon-password>

# Security
JWT_SECRET=<strong-random-secret-min-32-chars>

# Cloudinary (same cloud as local)
CLOUDINARY_CLOUD_NAME=vimeanbaby
CLOUDINARY_API_KEY=<api_key>
CLOUDINARY_API_SECRET=<api_secret>

# CORS (Vercel URLs)
CORS_ALLOWED_ORIGINS=https://vimeanbaby.vercel.app,https://vimeanbaby-dashboard.vercel.app

# Admin seed
ADMIN_EMAIL=admin@vimeanbaby.com
ADMIN_PASSWORD=<strong-password>
ADMIN_FULL_NAME=Vimean Baby Admin
```

---

## 📡 API Reference

Full interactive docs available at `/swagger-ui.html` when the backend is running.

| Scope | Example Endpoints |
|---|---|
| **Public** | `GET /api/public/products` · `GET /api/public/products/{slug}` · `GET /api/public/categories` |
| **Auth** | `POST /api/auth/register` · `POST /api/auth/login` · `GET /api/auth/me` |
| **Customer** | `GET /api/me/orders` · `CRUD /api/me/addresses` · `POST /api/me/orders/{id}/cancel` |
| **Admin** | `CRUD /api/admin/products` · `PATCH /api/admin/orders/{id}/status` · `GET /api/admin/dashboard/summary` |
| **Media** | `POST /api/admin/media/upload` → Cloudinary |

**Response envelope:**

```json
{ "success": true,  "data": { ... }, "message": null }
{ "success": false, "data": null,    "message": "Error description", "errors": [...] }
```

See [`docs/api-endpoints.md`](docs/api-endpoints.md) for the complete endpoint reference.

---

## 🎨 Design System

Vimean Baby uses a **coral pink + cream** palette with full Tailwind token coverage.

| Role | Hex | Tailwind token |
|---|---|---|
| Primary (CTA buttons) | `#FF2E51` | `bg-brand` |
| Highlight / links | `#FF8297` | `text-brand-light` |
| Soft section tint | `#FFE6EA` | `bg-brand-tint` |
| Badge pink | `#F4A6B8` | `bg-blush` |
| Success / authentic | `#8FD3B6` | `bg-mint` |
| Page background | `#FFF9F0` | `bg-cream` |
| Cards / forms | `#FFFFFF` | `bg-white` |
| Main text | `#2B3A4A` | `text-ink` |
| Muted text | `#6B7785` | `text-ink-muted` |
| Error | `#E5534B` | `text-danger` |
| Warning / low stock | `#F0A030` | `text-warn` |

**60/30/10 rule:** cream + white / soft pink + mint tints / coral pink buttons + accents.

See [`docs/design-palette.md`](docs/design-palette.md) for full usage guidelines.

---

## ☁️ Deployment

### Backend → Railway (Docker)

The `backend/` directory is fully configured for Railway via Docker.

| File | Purpose |
|---|---|
| `Dockerfile` | Multi-stage Java 21 image |
| `railway.toml` | Health check path `/actuator/health` |

**Step-by-step:**

1. Push this repository to GitHub.
2. Go to [Railway](https://railway.app) → **New Project** → **Deploy from GitHub** → select this repo.
3. Under Service settings, set **Root Directory** to `backend`.
4. Set **Builder** to `Dockerfile` (or leave auto; `railway.toml` handles it).
5. Under **Settings → Networking**, generate a public domain.
6. Add all production env vars from the [Environment Variables](#-environment-variables) section in the **Variables** tab.
7. Redeploy — Flyway migrations run automatically on first boot.
8. Verify: `https://<your-domain>/actuator/health` → `{"status":"UP"}`

> See [`docs/railway-backend.md`](docs/railway-backend.md) for a detailed walkthrough.

### Frontends → Vercel

Both `user-frontend/` and `admin-frontend/` deploy as separate Vercel projects.

1. Import each folder as an independent Vercel project.
2. Set the **Root Directory** to `user-frontend` or `admin-frontend`.
3. Add `NUXT_PUBLIC_API_BASE=https://<your-railway-domain>/api` as an environment variable.
4. Deploy — Vercel detects Nuxt automatically.

After both frontends are live, update `CORS_ALLOWED_ORIGINS` on Railway with the Vercel URLs and redeploy.

---

## 🗺️ Development Roadmap

| Phase | Description | Status |
|---|---|---|
| **Phase 0** | Monorepo scaffold & tooling | ✅ Complete |
| **Phase 1** | Database schema & catalog backend | ✅ Complete |
| **Phase 2** | Authentication (JWT) | 🔲 In Progress |
| **Phase 3** | Cloudinary media management | ✅ Complete |
| **Phase 4** | Customer storefront MVP (catalog & browse) | ✅ Complete |
| **Phase 5** | Cart & checkout | 🔲 Planned |
| **Phase 6** | Customer account & order history | 🔲 Planned |
| **Phase 7** | Admin panel (products, orders, customers) | 🔲 Planned |
| **Phase 8** | Payments (ABA PayWay, KHQR) & shipping APIs | 🔲 Planned |
| **Phase 9** | Production deployment & monitoring | 🔲 Planned |

---

## 🤝 Contributing

This is a private project. Contributions are by invitation only.

**Development conventions:**
- Backend: package by feature; DTOs with MapStruct; `GlobalExceptionHandler` for all errors; Flyway for all schema changes — never edit a previously applied migration.
- Frontend: Pinia for state; composables for API calls; i18n keys for all user-facing strings.
- Money: always `BigDecimal` / `NUMERIC(12,2)` — never `float` or `double`.
- Secrets: read from environment variables — never hardcode or commit.

---

## 📄 License

**All rights reserved.**  
© 2026 Vimean Baby. Unauthorized use, reproduction, or distribution is strictly prohibited.
