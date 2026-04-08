# TrackerTech

**Field service technician tracking platform** — complete solution for managing field technicians, job sites, and work orders with real-time GPS verification and offline-first mobile support.

## Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   Mobile App    │     │   Admin Dashboard │     │    Backend API   │
│   (Flutter)     │────>│   (Angular)       │────>│  (Spring Boot)   │
│                 │     │                   │     │                  │
│  • Technicians  │     │  • Admins         │     │  • PostgreSQL    │
│  • Offline-first│     │  • Real-time chat │     │  • JWT Auth      │
│  • GPS + Camera │     │  • Analytics      │     │  • WebSocket     │
└─────────────────┘     └──────────────────┘     └─────────────────┘
```

## Repositories

| Repo | Stack | Description |
|------|-------|-------------|
| [`trackertech-backend`](https://github.com/YOUR_USERNAME/trackertech-backend) | Spring Boot · Kotlin · PostgreSQL | REST API + WebSocket + File uploads |
| [`trackertech-mobile`](https://github.com/YOUR_USERNAME/trackertech-mobile) | Flutter · Dart · SQLite | Technician mobile app (Android/iOS) |
| [`trackertech-dashboard`](https://github.com/YOUR_USERNAME/trackertech-dashboard) | Angular · TypeScript · Tailwind | Admin web dashboard |

## Features

### For Technicians (Mobile App)
- Create work sites with GPS coordinates + camera photos
- Start/pause/resume/complete jobs with live timer
- GPS verification at job start and completion
- Before/after photo capture and upload
- Materials tracking per job
- In-app chat with admin
- Offline mode — works without internet, syncs when connected
- Biometric unlock (fingerprint/face)
- Persistent notification with job timer

### For Admins (Dashboard)
- Approve/suspend technician accounts
- View all sites, jobs, and materials
- Real-time chat with technicians
- Job analytics (7-day trend chart, live active jobs)
- Export data as CSV (technicians, sites, jobs)
- Assign technicians to sites
- View before/after photos and GPS verification

### Backend
- JWT authentication with role-based access (ADMIN/TECHNICIAN)
- Full REST API (28 endpoints)
- WebSocket chat (STOMP over SockJS)
- Bidirectional sync engine (push/pull with timestamps)
- File upload with local storage
- Flyway database migrations
- Soft delete on all entities

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Spring Boot 3.4 · Kotlin 2.1 · PostgreSQL 16 · Flyway |
| Auth | JWT (jjwt) · BCrypt · Spring Security |
| Real-time | WebSocket · STOMP · SockJS |
| Mobile | Flutter 3.18 · Riverpod · GoRouter · sqflite · Dio |
| Dashboard | Angular 21 · Tailwind CSS · Chart.js · Lucide Icons |
| Maps | OpenStreetMap (flutter_map + Leaflet) |
| Storage | Local filesystem (configurable for S3/MinIO) |
| Crash Reporting | Firebase Crashlytics |

## Quick Start

### Prerequisites
- Java 21+
- Node.js 20+
- Flutter 3.18+
- PostgreSQL 16 (or Docker)
- Android Studio (for emulator)

### 1. Backend
```bash
cd trackertech-backend
docker compose up -d          # Start PostgreSQL
./gradlew bootRun             # Start API on :8080
```

### 2. Dashboard
```bash
cd trackertech-dashboard
npm install
ng serve                      # Start on :4200
```

### 3. Mobile
```bash
cd trackertech-mobile
flutter pub get
flutter run                   # Run on connected device/emulator
```

### First-time Setup
1. Open `http://localhost:4200` → Bootstrap screen creates the first admin account
2. Open the mobile app → Register as a technician
3. Admin dashboard → Approvals → Approve the technician
4. Mobile app → Check Status → Start using the app

## Project Status

| Phase | Description | Status |
|-------|------------|--------|
| 1 | Backend API | ✅ Complete |
| 2 | Admin Dashboard | ✅ Complete |
| 3 | Mobile App | ✅ Complete |
| 4 | Photo Upload | ✅ Complete |
| 5 | Sync Engine | ✅ Complete |
| 6 | Chat System | ✅ Complete |
| 7 | Push Notifications | 🔲 Planned |
| 8 | Production Polish | ✅ Complete |
| 9 | Deployment | 🔲 Planned |

## Repositories

| Repo | Visibility | Purpose |
|------|-----------|---------|
| **trackertech** (this repo) | Public | Full monorepo — backend + dashboard + mobile |
| **trackertech-mobile** | Private | Flutter app for Codemagic iOS/Android builds |
| **trackertech-dashboard** | Private | Angular admin for Vercel/Netlify deploy |
| **trackertech-backend** | Private | Spring Boot API for server deploy |

## License

MIT
