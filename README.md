# Ecommerce Platform

> Full-stack commerce platform with a Django REST API, a React PWA storefront, Stripe payments and ML-powered sales forecasting.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-5.2-092E20?style=flat-square&logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/Django%20REST%20Framework-A30000?style=flat-square&logo=django&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-635BFF?style=flat-square&logo=stripe&logoColor=white)
![Cloudinary](https://img.shields.io/badge/Cloudinary-3448C5?style=flat-square&logo=cloudinary&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react&logoColor=black)
![Vite](https://img.shields.io/badge/Vite-7-646CFF?style=flat-square&logo=vite&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)
![PWA](https://img.shields.io/badge/PWA-ready-5A0FC8?style=flat-square&logo=pwa&logoColor=white)

## Overview

Small and mid-sized retailers often run their catalogue, orders, payments and demand planning in separate tools. **Ecommerce Platform** combines them into one product:

- a **Django REST API** for users, roles, catalogue, cart, orders, payments and AI endpoints;
- a **React PWA storefront** with a customer catalogue, cart, checkout and admin dashboards;
- **Stripe** for checkout and webhooks;
- **Cloudinary** for product media;
- **scikit-learn** models for sales and order forecasting.

## Architecture

```text
ecommerce-platform/
├── backend/    Django 5.2 + DRF + SimpleJWT + Stripe + Cloudinary + scikit-learn
└── frontend/   React 18 + Vite 7 + Tailwind CSS 4 + Stripe.js + PWA
```

```text
React PWA ──REST / JWT──► Django REST API ──► PostgreSQL
                               ├──► Stripe (checkout + webhooks)
                               ├──► Cloudinary (media)
                               └──► scikit-learn models (.pkl)
```

## Modules

| Module | Stack | Responsibility |
|---|---|---|
| `backend/` | Python, Django 5.2, DRF, SimpleJWT, drf-yasg, Stripe, Cloudinary, scikit-learn, pandas, NumPy | Authentication, roles and permissions, catalogue, cart, orders, payments, forecasting |
| `frontend/` | React 18, Vite 7, Tailwind CSS 4, React Router 7, Stripe.js, PWA | Storefront, product catalogue, cart, checkout, admin and permission screens |

### Backend apps

| App | Responsibility |
|---|---|
| `usuario` | Registration, login, JWT, profiles, roles, groups and privileges |
| `producto` | Categories, subcategories, brands, products and NLP-assisted search |
| `venta` | Cart, orders, payment methods and Stripe checkout/webhooks |
| `ia` | Sales and order forecasting with Random Forest models |

## Getting started

### Prerequisites

- Python 3.11+
- Node.js 20+
- PostgreSQL 14+
- Stripe test keys
- Cloudinary account
- Optional: Google Gemini API key for AI-assisted features

### 1. Backend

```bash
cd backend
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env             # fill database, Stripe, Cloudinary and AI values
python manage.py migrate
python manage.py runserver
```

API: `http://localhost:8000`  
Swagger UI: `http://localhost:8000/swagger/`  
ReDoc: `http://localhost:8000/redoc/`

### 2. Frontend

```bash
cd frontend
npm ci
cp .env.example .env             # VITE_API_URL=http://127.0.0.1:8000/api
npm run dev
```

App: `http://localhost:5173`

### 3. ML models

The repository includes trained Random Forest models under `backend/ia/ml/`. To retrain them:

```bash
cd backend
python manage.py generar_historico_precios
python manage.py entrenar_modelo_ventas
```

## Main API areas

| Prefix | Purpose |
|---|---|
| `/usuario/` | Auth, JWT, profiles, users, roles, groups and privileges |
| `/producto/` | Categories, subcategories, brands, products, search |
| `/venta/` | Cart, orders, payment methods, Stripe checkout and webhooks |
| `/ia/` | Sales prediction and monthly sales prediction |

## Environment variables

Copy `backend/.env.example` and `frontend/.env.example`. Never commit real values.

| Module | Key variables |
|---|---|
| `backend` | `SECRET_KEY`, `DEBUG`, `ALLOWED_HOSTS`, `DB_*`, `STRIPE_*`, `CLOUDINARY_*`, `FRONTEND_URL`, `API_GEMINI`, `FIREBASE_CONFIG` |
| `frontend` | `VITE_API_URL` |

## Security

- Settings are environment-driven; the Django `SECRET_KEY` is not committed.
- Stripe and Cloudinary credentials are read from the environment.
- The original repository history was sanitized before publication.
- Use HTTPS, secure cookies and restricted CORS in production.

## A note on provenance

This monorepo consolidates the former `ECOMMERCE_BACK` and `ECOMMERCE_FRONT` repositories. Full commit history is preserved with `git subtree`.
