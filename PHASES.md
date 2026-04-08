# TrackerTech — Project Phases

## Phase 1: Backend Foundation ✅ DONE
**Goal:** REST API with auth, CRUD, and job lifecycle

| Task | Status |
|------|--------|
| Spring Boot + Kotlin project setup | ✅ |
| PostgreSQL + Flyway migrations (profiles, sites, jobs, materials, job_events, job_materials) | ✅ |
| JWT auth (login, register, bootstrap) | ✅ |
| User management (CRUD, role/status updates) | ✅ |
| Site management (CRUD, assign technician) | ✅ |
| Job lifecycle (start → pause → resume → complete) | ✅ |
| Job events timeline | ✅ |
| Materials (CRUD, link to jobs) | ✅ |
| GPS verification fields (locationVerified, locationDistanceM, fraudScore) | ✅ |
| Soft delete (serverDeletedAt) on all entities | ✅ |
| CORS config for frontend | ✅ |
| Allow technicians to create + self-assign sites | ✅ |

---

## Phase 2: Admin Dashboard (Angular) ✅ DONE
**Goal:** Web app for admins to manage technicians, sites, jobs

| Task | Status |
|------|--------|
| Angular 21 + Tailwind CSS + dark theme | ✅ |
| Login + Bootstrap (first admin) screens | ✅ |
| Dashboard (stats, live timer strip, 7-day chart) | ✅ |
| Technicians (list, detail, add modal, status actions) | ✅ |
| Approvals (pending technicians card grid) | ✅ |
| Sites (list, detail with OpenStreetMap, assign tech) | ✅ |
| Jobs (list, detail with timeline + before/after photos) | ✅ |
| Materials (list, add with unit select) | ✅ |
| Settings (display, notifications, export, about) | ✅ |
| Profile (edit name, change password) | ✅ |
| Responsive mobile layout (sidebar, tables, filters) | ✅ |
| Route guards (auth, guest) | ✅ |
| HTTP interceptor (JWT Bearer) | ✅ |

---

## Phase 3: Mobile App (Flutter) ✅ DONE
**Goal:** Technician-facing app to create sites, track jobs, take photos

| Task | Status |
|------|--------|
| **Core Infrastructure** | |
| Dio HTTP client + auth interceptor | ✅ |
| Secure token storage (FlutterSecureStorage) | ✅ |
| Dark theme matching admin dashboard | ✅ |
| GoRouter with auth redirects | ✅ |
| Domain exceptions + Dio error mapping | ✅ |
| Logger (terminal) + Toast (user-facing) | ✅ |
| Repository interfaces (IAuthRepo, IJobRepo, etc.) | ✅ |
| Environment config (--dart-define for API URL) | ✅ |
| Immutable models (@immutable) | ✅ |
| **Auth** | |
| Login screen | ✅ |
| Register screen | ✅ |
| Pending approval screen | ✅ |
| 403 handling → redirect to pending | ✅ |
| **Navigation** | |
| Bottom bar: Home + Sites (center FAB) + Profile | ✅ |
| Curved notch BottomAppBar | ✅ |
| Page transition animations (slide, fade, scale) | ✅ |
| **Screens** | |
| Splash screen (animated TT logo) | ✅ |
| Onboarding (3 swipeable intro slides) | ✅ |
| Home (greeting, active job card, my sites, recent jobs) | ✅ |
| Sites list (search, map toggle, New Site FAB) | ✅ |
| Create site (auto GPS + reverse geocode address + camera photo) | ✅ |
| Site detail (info, location, Start Job button) | ✅ |
| Sites map view (OpenStreetMap, markers, selection card) | ✅ |
| Job detail (live timer, pause/resume, materials) | ✅ |
| Complete job (camera photo, notes, GPS re-check) | ✅ |
| Job history (search/filter) | ✅ |
| Add material bottom sheet | ✅ |
| Profile (stats, job history link, settings link, logout) | ✅ |
| Settings (notifications, GPS, about) | ✅ |
| **Offline** | |
| Connectivity detection (periodic check) | ✅ |
| Offline banner (animated slide-in) | ✅ |
| Cache service (save/read API responses) | ✅ |
| Sites + Jobs cache fallback when offline | ✅ |
| **Notifications** | |
| Persistent job timer notification (Android) | ✅ |

---

## Phase 4: Photo Upload + Storage ✅ DONE
**Goal:** Upload before/after photos to server, display in admin dashboard

| Task | Status |
|------|--------|
| Backend: Local file storage (./uploads directory) | ✅ |
| Backend: POST /api/upload endpoint (multipart, 10MB max) | ✅ |
| Backend: Serve uploaded files via /uploads/** (public, no auth) | ✅ |
| Backend: CreateSiteRequest accepts beforePhotoUrl | ✅ |
| Mobile: Upload service with progress callback | ✅ |
| Mobile: Create site uploads before photo | ✅ |
| Mobile: Complete job uploads after photo | ✅ |
| Mobile: Upload progress % shown on button | ✅ |
| Mobile: Auto-logout on token expiry (401 → force logout + toast) | ✅ |
| Mobile: Save photo locally if offline, upload when online | 🔲 (Phase 5) |
| Admin: Display before/after photos in job detail | ✅ (already reads URLs) |
| Admin: Display site before photo in site detail | ✅ (already reads URLs) |

---

## Phase 5: Sync Engine ✅ DONE
**Goal:** Full offline-first with bidirectional sync

| Task | Status |
|------|--------|
| Backend: POST /api/sync/pull (delta since lastSyncedAt) | ✅ |
| Backend: POST /api/sync/push (batch operations: CREATE_SITE, START_JOB, COMPLETE_JOB, ADD_MATERIAL) | ✅ |
| Backend: Conflict resolution (server-wins, error reporting per op) | ✅ |
| Backend: Repository queries (findAllByLastModifiedAtAfter) | ✅ |
| Mobile: sqflite local database (sites, jobs, materials, pending_ops, sync_meta) | ✅ |
| Mobile: Pending operations queue (offline create site, start job, complete job) | ✅ |
| Mobile: Sync service (push pending → pull delta → upsert local) | ✅ |
| Mobile: Sync status indicator (syncing banner) | ✅ |
| Mobile: Providers read from local DB when offline, API when online | ✅ |
| Mobile: Automatic fallback to local DB on API failure | ✅ |

---

## Phase 6: Chat + Offline Jobs ✅ DONE
**Goal:** In-app chat between tech and admin, offline job creation

| Task | Status |
|------|--------|
| **Offline Jobs** | |
| Mobile: Save job locally in SQLite when starting offline | ✅ |
| Mobile: Queue operation for sync | ✅ |
| Mobile: Job appears in UI immediately from local DB | ✅ |
| **Chat Backend** | |
| Backend: chat_messages DB table (Flyway V2 migration) | ✅ |
| Backend: ChatMessage entity + ChatRepository | ✅ |
| Backend: ChatService (rooms per tech, send, read, history) | ✅ |
| Backend: REST API (GET rooms, GET/POST messages, POST read) | ✅ |
| Backend: WebSocket STOMP config (/ws endpoint) | ✅ |
| Backend: WebSocket chat controller (real-time broadcast) | ✅ |
| **Chat Mobile** | |
| Mobile: Chat screen (message bubbles, input, auto-scroll) | ✅ |
| Mobile: STOMP WebSocket connection for real-time updates | ✅ |
| Mobile: Chat link in profile menu | ✅ |
| **Chat Admin** | |
| Admin: Chat page with room sidebar + message area | ✅ |
| Admin: Room list with unread badge counts | ✅ |
| Admin: Real-time polling (3s) for new messages | ✅ |
| Admin: Chat nav item in sidebar | ✅ |
| Admin: Responsive (mobile: room list → chat view) | ✅ |

## Phase 7: Push Notifications 🔲 NOT STARTED
**Goal:** Firebase push notifications for approvals, chat messages, assignments

| Task | Status |
|------|--------|
| Firebase Cloud Messaging setup (Android + iOS) | 🔲 |
| Backend: Firebase Admin SDK integration | 🔲 |
| Backend: Save device FCM tokens per user | 🔲 |
| Backend: Send notification on chat message | 🔲 |
| Backend: Send notification on approval | 🔲 |
| Backend: Send notification on site assignment | 🔲 |
| Mobile: FCM token registration on login | 🔲 |
| Mobile: Handle foreground + background notifications | 🔲 |
| Mobile: Deep link from notification → relevant screen | 🔲 |

---

## Phase 8: Production Polish ✅ DONE
**Goal:** Production-ready quality

| Task | Status |
|------|--------|
| Backend: Profile update (PATCH /api/users/me) | ✅ |
| Backend: Password change (POST /api/users/me/password) | ✅ |
| Backend: CSV export (GET /api/export/technicians, /jobs, /sites) | ✅ |
| Mobile: Biometric login (fingerprint/face unlock) | ✅ |
| Mobile: Biometric toggle in Settings | ✅ |
| Mobile: Firebase Crashlytics (auto crash reporting) | ✅ |
| Mobile: Native splash screen (dark background) | ✅ |
| App signing (Android keystore, iOS provisioning) | 🔲 Phase 9 |
| Unit tests | 🔲 Future |
| Widget tests | 🔲 Future |

---

## Phase 8: Deployment 🔲 NOT STARTED
**Goal:** Ship it

| Task | Status |
|------|--------|
| Backend: Dockerize (Dockerfile + docker-compose.prod.yml) | 🔲 |
| Backend: Deploy to VPS/cloud (DigitalOcean, AWS, etc.) | 🔲 |
| Backend: SSL certificate (Let's Encrypt) | 🔲 |
| Backend: Environment variables for production | 🔲 |
| Admin: Build + deploy to Nginx / Vercel / Netlify | 🔲 |
| Mobile: Build release APK | 🔲 |
| Mobile: Google Play Store listing | 🔲 |
| Mobile: Apple App Store listing (if iOS) | 🔲 |
| DNS + domain setup | 🔲 |
| Monitoring (uptime, logs, alerts) | 🔲 |

---

## Summary

| Phase | Description | Status |
|-------|------------|--------|
| 1 | Backend (Spring Boot API) | ✅ Done |
| 2 | Admin Dashboard (Angular) | ✅ Done |
| 3 | Mobile App (Flutter) | ✅ Done |
| 4 | Photo Upload + Storage | ✅ Done |
| 5 | Sync Engine (full offline) | ✅ Done |
| 6 | Chat + Offline Jobs | ✅ Done |
| 7 | Push Notifications | 🔲 |
| 8 | Production Polish | ✅ Done |
| 9 | Deployment | 🔲 |
