<div align="center">

# TrackerTech

### Field Service Technician Tracking Platform

[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.4-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![Kotlin](https://img.shields.io/badge/Kotlin-2.1-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![Angular](https://img.shields.io/badge/Angular-21-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://angular.dev)
[![Flutter](https://img.shields.io/badge/Flutter-3.18-02569B?style=for-the-badge&logo=flutter&logoColor=white)](https://flutter.dev)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org)
[![Tailwind](https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)

A complete full-stack platform for managing field technicians, work sites, and job orders with real-time GPS verification, offline-first mobile support, and in-app chat.

[Backend API](#backend-api) · [Admin Dashboard](#admin-dashboard) · [Mobile App](#mobile-app) · [Quick Start](#quick-start)

---

</div>

## Architecture

```
┌──────────────────────┐         ┌──────────────────────┐         ┌──────────────────────┐
│                      │         │                      │         │                      │
│     Mobile App       │  HTTP   │    Admin Dashboard   │  HTTP   │     Backend API       │
│    Flutter · Dart    │────────>│   Angular · Tailwind │────────>│  Spring Boot · Kotlin │
│                      │         │                      │         │                      │
│  - Technician-facing │         │  - Admin-facing      │         │  - PostgreSQL 16      │
│  - Offline-first     │         │  - Real-time chat    │         │  - JWT Auth           │
│  - GPS + Camera      │         │  - Charts + Export   │         │  - WebSocket (STOMP)  │
│  - Biometric unlock  │         │  - WCAG AA a11y      │         │  - File uploads       │
│                      │         │                      │         │  - Sync engine        │
└──────────────────────┘         └──────────────────────┘         └──────────────────────┘
```

## Tech Stack

<table>
<tr>
<td width="33%" valign="top">

### Backend API
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)](https://spring.io)
[![Kotlin](https://img.shields.io/badge/Kotlin-7F52FF?style=flat-square&logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)](https://postgresql.org)
[![JWT](https://img.shields.io/badge/JWT-000000?style=flat-square&logo=jsonwebtokens&logoColor=white)](https://jwt.io)

- Spring Security + BCrypt
- Flyway migrations
- STOMP WebSocket
- Multipart file uploads
- CSV export

</td>
<td width="33%" valign="top">

### Admin Dashboard
[![Angular](https://img.shields.io/badge/Angular-DD0031?style=flat-square&logo=angular&logoColor=white)](https://angular.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://typescriptlang.org)
[![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![Chart.js](https://img.shields.io/badge/Chart.js-FF6384?style=flat-square&logo=chartdotjs&logoColor=white)](https://chartjs.org)

- Standalone components + Signals
- Dark theme with CSS variables
- Lucide SVG icons
- Responsive (mobile + desktop)
- WCAG AA accessible

</td>
<td width="33%" valign="top">

### Mobile App
[![Flutter](https://img.shields.io/badge/Flutter-02569B?style=flat-square&logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-0175C2?style=flat-square&logo=dart&logoColor=white)](https://dart.dev)
[![Firebase](https://img.shields.io/badge/Firebase-DD2C00?style=flat-square&logo=firebase&logoColor=white)](https://firebase.google.com)
[![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white)](https://sqlite.org)

- Riverpod state management
- GoRouter with auth guards
- Offline-first (sqflite)
- GPS + camera + biometric
- Firebase Crashlytics

</td>
</tr>
</table>

## Features

<table>
<tr>
<td width="50%" valign="top">

### For Technicians
| Feature | Details |
|:--------|:--------|
| **Create Sites** | GPS auto-capture + reverse geocode + camera photo |
| **Start Jobs** | One-tap start with location verification |
| **Live Timer** | Persistent notification, pause/resume |
| **Photo Capture** | Before/after photos with upload |
| **Materials** | Track materials used per job |
| **Chat** | Real-time messaging with admin |
| **Offline Mode** | Full offline support, auto-sync on reconnect |
| **Biometric** | Fingerprint/face unlock |
| **Map View** | All sites on OpenStreetMap |

</td>
<td width="50%" valign="top">

### For Admins
| Feature | Details |
|:--------|:--------|
| **Manage Techs** | Approve, suspend, activate accounts |
| **Dashboard** | Live stats, 7-day chart, active job strip |
| **Chat** | Message any technician, read receipts |
| **Sites** | Map view, assign technicians |
| **Jobs** | Timeline, GPS verification, photos |
| **Materials** | Catalog management |
| **CSV Export** | Download technicians, jobs, sites |
| **Role-based** | Admin vs technician permissions |

</td>
</tr>
</table>

## API Overview

<details>
<summary><b>28 REST Endpoints + WebSocket</b> — click to expand</summary>

| Group | Endpoints | Auth |
|:------|:----------|:-----|
| **Auth** | `POST /auth/bootstrap` · `/register` · `/login` | Public |
| **Users** | `GET /users/me` · `PATCH /users/me` · `POST /users/me/password` | Any |
| **Users (Admin)** | `GET /users` · `/{id}` · `PATCH /{id}/status` · `/{id}/role` · `DELETE` | Admin |
| **Sites** | `GET /sites` · `/{id}` · `POST` · `PATCH /{id}/assign` · `DELETE` | Any |
| **Jobs** | `GET /jobs` · `/my` · `/{id}` · `POST /start` · `PATCH /pause` · `/resume` · `/complete` | Any/Tech |
| **Materials** | `GET /materials` · `POST` · `DELETE` · `GET /job/{id}` · `POST /job/{id}` | Any |
| **Chat** | `GET /rooms` · `/my-room` · `GET /rooms/{id}/messages` · `POST /messages` · `/read` | Any |
| **Sync** | `POST /sync/pull` · `/sync/push` | Any |
| **Upload** | `POST /upload` (multipart) | Any |
| **Export** | `GET /export/technicians` · `/sites` · `/jobs` (CSV) | Admin |
| **WebSocket** | `ws://host/ws` — `/topic/chat/{roomId}` | STOMP |

</details>

## Quick Start

### Prerequisites

![Java](https://img.shields.io/badge/Java-21+-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Node](https://img.shields.io/badge/Node.js-20+-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![Flutter](https://img.shields.io/badge/Flutter-3.18+-02569B?style=flat-square&logo=flutter&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-optional-2496ED?style=flat-square&logo=docker&logoColor=white)

### 1. Backend
```bash
cd backend
docker compose up -d        # Start PostgreSQL
./gradlew bootRun           # API on http://localhost:8080
```

### 2. Dashboard
```bash
cd frontend
npm install
ng serve                    # Dashboard on http://localhost:4200
```

### 3. Mobile
```bash
cd mobile
flutter pub get
flutter run                 # Launch on device/emulator
```

### 4. First-time Setup
1. Open `localhost:4200` — create admin account (Bootstrap screen)
2. Mobile app — Register as technician
3. Dashboard — Approvals — Approve the tech
4. Mobile — Check Status — Start using the app

## Project Structure

```
trackertech/
├── backend/                 # Spring Boot API (Kotlin)
│   ├── src/main/kotlin/     # Controllers, services, entities
│   ├── src/main/resources/  # application.yml, Flyway migrations
│   └── build.gradle.kts
│
├── frontend/                # Angular Admin Dashboard
│   ├── src/app/features/    # Dashboard, chat, technicians, jobs...
│   ├── src/styles.scss      # Dark theme + CSS variables
│   └── angular.json
│
├── mobile/                  # Flutter Mobile App
│   ├── lib/core/            # Network, DB, services, theme
│   ├── lib/features/        # Auth, jobs, sites, chat, profile...
│   └── pubspec.yaml
│
├── PHASES.md                # Development roadmap
└── DEPLOYMENT.md            # Deployment guide
```

## Project Status

| Phase | Description | Status |
|:-----:|:------------|:------:|
| 1 | Backend API | Done |
| 2 | Admin Dashboard | Done |
| 3 | Mobile App | Done |
| 4 | Photo Upload | Done |
| 5 | Sync Engine | Done |
| 6 | Chat System | Done |
| 7 | Push Notifications | Planned |
| 8 | Production Polish | Done |
| 9 | Deployment | Planned |

<details>
<summary><b>Environment Variables</b></summary>

### Backend
| Variable | Default | Description |
|:---------|:--------|:------------|
| `DB_URL` | `jdbc:postgresql://localhost:5432/trackertech` | Database URL |
| `DB_USER` | `postgres` | Database user |
| `DB_PASS` | `postgres` | Database password |
| `JWT_SECRET` | `change-me-in-production` | JWT signing key |
| `UPLOAD_DIR` | `./uploads` | Photo storage path |

### Mobile
| Variable | Default | Description |
|:---------|:--------|:------------|
| `API_URL` | `http://10.0.2.2:8080/api` | Backend URL (via `--dart-define`) |

</details>

<details>
<summary><b>Database Schema</b></summary>

| Table | Description |
|:------|:-----------|
| `profiles` | Users (admin + technicians) |
| `sites` | Work locations with GPS coordinates |
| `jobs` | Work orders with lifecycle tracking |
| `job_events` | Job timeline events |
| `materials` | Material catalog |
| `job_materials` | Materials used per job |
| `chat_messages` | Chat messages between tech and admin |

All tables include `created_at`, `last_modified_at`, and soft delete via `server_deleted_at`.

</details>

## Repositories

| Repo | Visibility | Purpose |
|:-----|:----------:|:--------|
| **trackertech** *(this)* | Public | Full monorepo |
| **trackertech-mobile** | Private | Flutter — Codemagic CI/CD |
| **trackertech-dashboard** | Private | Angular — Vercel deploy |
| **trackertech-backend** | Private | Spring Boot — Server deploy |

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built with Spring Boot · Angular · Flutter**

</div>
