# TrackerTech — GitHub & Deployment Guide

## Repo Structure

```
GitHub:
├── trackertech-backend    ← Spring Boot API (deploy to server)
├── trackertech-mobile     ← Flutter app (Codemagic for iOS/Android builds)
└── trackertech-dashboard  ← Angular admin (deploy to Vercel/Netlify)
```

---

## Step 1: Create Repos on GitHub

Go to https://github.com/new and create 3 **private** repos:
- `trackertech-backend`
- `trackertech-mobile`
- `trackertech-dashboard`

Don't initialize with README (we'll push existing code).

---

## Step 2: Push Backend

```bash
cd "C:\Users\wizli\Documents\tracker-tech\tracker-tech-v1\backend"
git init
git add .
git commit -m "Initial commit — Spring Boot API with auth, sites, jobs, materials, chat, sync, upload, export"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/trackertech-backend.git
git push -u origin main
```

---

## Step 3: Push Mobile (Flutter)

```bash
cd "C:\Users\wizli\Documents\tracker-tech\tracker-tech-v1\mobile"
git init
git add .
git commit -m "Initial commit — Flutter app with offline sync, chat, biometric, maps, notifications"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/trackertech-mobile.git
git push -u origin main
```

---

## Step 4: Push Dashboard (Angular)

```bash
cd "C:\Users\wizli\Documents\tracker-tech\tracker-tech-v1\frontend"
git init
git add .
git commit -m "Initial commit — Angular admin dashboard with chat, a11y, responsive design"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/trackertech-dashboard.git
git push -u origin main
```

---

## Step 5: Codemagic Setup (iOS & Android Builds)

1. Go to https://codemagic.io → Sign in with GitHub
2. Add the `trackertech-mobile` repo
3. Codemagic auto-detects Flutter at the repo root
4. Configure build settings:
   - **Android**: Codemagic builds debug APK automatically, for release you need a keystore
   - **iOS**: Requires Apple Developer account ($99/year)
   - Codemagic can manage iOS code signing automatically via "Apple Developer Portal integration"
5. Trigger a build → get `.apk` (Android) or `.ipa` (iOS TestFlight)

### Codemagic Environment Variables (if needed)

If you gitignored `google-services.json`, add it as a Codemagic secret:
- Go to App settings → Environment variables
- Add `GOOGLE_SERVICES_JSON` with the file contents (base64 encoded)
- Add a pre-build script to decode it

---

## Step 6: Deploy Backend to Production

### Option A: DigitalOcean / VPS

```bash
# On your server (Ubuntu)
sudo apt update && sudo apt install docker.io docker-compose -y

# Clone the repo
git clone https://github.com/YOUR_USERNAME/trackertech-backend.git
cd trackertech-backend

# Create production docker-compose
# (see docker-compose.prod.yml below)

docker compose -f docker-compose.prod.yml up -d
```

### docker-compose.prod.yml

```yaml
version: '3.8'
services:
  db:
    image: postgres:16
    restart: always
    environment:
      POSTGRES_DB: trackertech
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - pgdata:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  api:
    build: .
    restart: always
    ports:
      - "8080:8080"
    environment:
      DB_URL: jdbc:postgresql://db:5432/trackertech
      DB_USER: postgres
      DB_PASS: ${DB_PASSWORD}
      JWT_SECRET: ${JWT_SECRET}
      UPLOAD_DIR: /app/uploads
    volumes:
      - uploads:/app/uploads
    depends_on:
      - db

volumes:
  pgdata:
  uploads:
```

### Dockerfile (add to backend root)

```dockerfile
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY build/libs/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Build before deploying

```bash
# On your local machine
cd backend
./gradlew bootJar

# Or on the server
./gradlew bootJar
docker compose -f docker-compose.prod.yml build
docker compose -f docker-compose.prod.yml up -d
```

---

## Step 7: SSL + Domain

```bash
# Install Certbot on server
sudo apt install certbot python3-certbot-nginx -y

# Get certificate
sudo certbot --nginx -d api.yourdomain.com

# Auto-renewal
sudo certbot renew --dry-run
```

Nginx config (`/etc/nginx/sites-available/trackertech`):

```nginx
server {
    listen 80;
    server_name api.yourdomain.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl;
    server_name api.yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/api.yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/api.yourdomain.com/privkey.pem;

    # API
    location /api/ {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # WebSocket
    location /ws/ {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    # Uploads
    location /uploads/ {
        proxy_pass http://localhost:8080;
    }
}
```

---

## Step 8: Deploy Admin Dashboard

### Option A: Vercel (easiest)

```bash
cd frontend
npm install -g vercel
vercel
# Follow prompts, select Angular framework
```

### Option B: Nginx on same server

```bash
cd frontend
ng build --configuration production

# Copy dist to server
scp -r dist/frontend/* user@server:/var/www/trackertech-admin/
```

Nginx config:
```nginx
server {
    listen 443 ssl;
    server_name admin.yourdomain.com;

    root /var/www/trackertech-admin/browser;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

---

## Step 9: Update API URLs for Production

### Mobile app
```bash
# Build with production URL
flutter run --dart-define=API_URL=https://api.yourdomain.com/api
flutter build apk --dart-define=API_URL=https://api.yourdomain.com/api
```

### Dashboard
Update `frontend/src/environments/environment.prod.ts`:
```typescript
export const environment = {
  production: true,
  apiUrl: 'https://api.yourdomain.com/api'
};
```

---

## Step 10: Build Release APK

```bash
cd mobile

# Generate signing key (one time only)
keytool -genkey -v -keystore trackertech.jks -keyalg RSA -keysize 2048 -validity 10000 -alias trackertech

# Create key.properties (DO NOT COMMIT)
cat > android/key.properties << EOF
storePassword=YOUR_STORE_PASSWORD
keyPassword=YOUR_KEY_PASSWORD
keyAlias=trackertech
storeFile=../../trackertech.jks
EOF

# Build release APK
flutter build apk --release --dart-define=API_URL=https://api.yourdomain.com/api

# Output: build/app/outputs/flutter-apk/app-release.apk
```

---

## Quick Reference

| What | Command |
|------|---------|
| Run backend locally | `cd backend && ./gradlew bootRun` |
| Run dashboard locally | `cd frontend && ng serve` |
| Run mobile on emulator | `cd mobile && flutter run -d emulator-5554` |
| Run mobile on real device | `cd mobile && flutter run` |
| Build release APK | `cd mobile && flutter build apk --release` |
| Build iOS | Use Codemagic or `cd mobile && flutter build ios --release` (macOS only) |
| Export CSV (admin) | `GET /api/export/technicians`, `/jobs`, `/sites` |
| Check backend health | `curl http://localhost:8080/api/auth/bootstrap` |

---

## Important Notes

- **Replace `YOUR_USERNAME`** with your actual GitHub username in all commands
- **Replace `yourdomain.com`** with your actual domain
- **Never commit** `key.properties`, `.jks` files, or `.env` with passwords
- **google-services.json** is safe to commit (contains public Firebase config, not secrets)
- **JWT_SECRET** in production should be a random 64+ character string
- **DB_PASSWORD** in production should be strong and unique
