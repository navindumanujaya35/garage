# Garage Management System

A complete, self-hosted web application for running **one** automobile garage: customers, vehicles and their
service history, appointments, job cards with inspections, spare-parts inventory, suppliers, quotations,
invoices, payments, expenses, reports and in-app notifications — usable from a computer, tablet or phone.

Simple by design: a monolith (FastAPI + React) with PostgreSQL, deployed with Docker Compose behind Nginx with
automatic HTTPS and daily backups.

## Quick start (production server)

```bash
git clone <repository-url> garage-management-system
cd garage-management-system
./scripts/init-env.sh          # or: cp .env.example .env  (then set the passwords)
nano .env                      # ENABLE_HTTPS=true, DOMAIN=..., LETSENCRYPT_EMAIL=...
docker compose up -d --build
```

Open `https://<your-domain>` and sign in as `admin` with the initial password (you will be asked to change it).
Full instructions: **[docs/DEPLOYMENT.md](docs/DEPLOYMENT.md)**.

## Features

| Area | What you can do |
|------|-----------------|
| Dashboard | Today's appointments, vehicles in the garage, open / waiting-approval / in-progress / completed-today jobs, today's revenue, outstanding payments, low stock, recent customers and job cards, charts |
| Customers & vehicles | Multiple vehicles per customer, fast search by name / phone / registration, full per-vehicle service history (work, parts, mileage, invoices, payments), ownership transfer |
| Appointments | Day / week / month calendar and list, statuses, convert to job card on arrival |
| Job cards | Mileage, complaints, condition, diagnosis, services / parts / labour / other charges, discount & tax, mechanic, priority, promised time, 12-stage workflow with guarded transitions, status history, printable job card |
| Inspections | Checklist (exterior, engine, tires, brakes, interior) with Pass / Attention / Fail / Not checked and notes |
| Inventory | Parts with stock & minimum levels, purchases, adjustments, returns, automatic deduction from job cards, low-stock alerts, full movement history |
| Suppliers | Contacts, parts supplied, last prices, purchase history |
| Quotations | From a job card or standalone; draft / sent / approved / rejected / expired; PDF; approval drives the job; convert to job card |
| Invoices & payments | Generated from job cards, server-side totals, partial payments, overpayment prevention, receipts, PDF, void instead of delete |
| Expenses | Categorised expenses (optionally created automatically from part purchases) |
| Reports | Sales (daily/monthly/yearly), jobs (completed, open, by service, by mechanic), customers (new, returning, top), vehicles, inventory (stock, low stock, usage, purchases), financial (revenue, expenses, profit estimate, outstanding) — export PDF / Excel / CSV |
| Admin | Garage settings (logo, currency, tax, numbering, working hours, invoice texts), users & roles (owner, office staff, mechanic), audit log |
| Platform | HTTPS (Let's Encrypt), secure sessions + CSRF, rate limiting, role-based access, structured logs, daily verified backups, restore & update scripts |

## Documentation

| Document | Contents |
|----------|----------|
| [docs/USER_GUIDE.md](docs/USER_GUIDE.md) | How to use the system day to day (start here if you work in the garage) |
| [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) | Install, HTTPS, configuration, backup, restore, moving servers, updating, development setup, credentials |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Architecture diagram, components, security model, workflows, requirements analysis & assumptions |
| [docs/DATABASE.md](docs/DATABASE.md) | ER diagram, tables, indexes, migrations |
| [docs/API.md](docs/API.md) | REST API overview and endpoint list (interactive docs at `/api/docs`) |
| [frontend/README.md](frontend/README.md) | Frontend structure and conventions |

## Repository layout

```
backend/            FastAPI app, SQLAlchemy models, Alembic migrations, tests, seed data
frontend/           React + TypeScript + Vite + Tailwind app
nginx/              Nginx image (serves the built frontend, proxies /api, TLS)
scripts/            backup / restore / update / init-env / certbot helpers
e2e/                Playwright end-to-end test (full job-card-to-payment flow)
docs/               documentation
docker-compose.yml  production stack: db, backend, web, certbot, backup
.env.example        configuration template
```

## Tech stack

Python 3.12 · FastAPI · SQLAlchemy 2 · Pydantic 2 · Alembic · PostgreSQL 16 · ReportLab · React 19 · TypeScript ·
Vite · Tailwind CSS 4 · Radix UI (shadcn-style components) · TanStack Query · Recharts · Nginx · Docker Compose.
