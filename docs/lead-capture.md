# Lead Capture System — Eshet Chayil

## Overview
Secure lead-capture pipeline for the Eshet Chayil landing page:
1. Form submits to server-side API proxy (`/api/lead`)
2. API validates, sanitizes, rate-limits, then writes to Directus
3. Directus Flow sends email notification to `hoshenyehuda@gmail.com`

---

## Components

### Directus Collection
- **Name:** `eshet_chayil_leads`
- **Fields:**
  - `id` (int, auto PK)
  - `created_at` (timestamp, default now())
  - `source` (varchar 100, default "eshet-chayil-landing")
  - `first_name` (varchar 100, required)
  - `last_name` (varchar 100, required)
  - `phone` (varchar 30, required)
  - `email` (varchar 255, required)
  - `city` (varchar 100, optional)
  - `marital_status` (varchar 30, optional)
  - `background` (text, optional)
  - `message` (text, optional)
  - `consent` (boolean, required)
  - `user_agent` (text, optional)
  - `ip` (varchar 45, optional)

### Directus Flow
- **ID:** `70f4241b-7be6-4888-a047-10c3c238474f`
- **Trigger:** `items.create` on `eshet_chayil_leads`
- **Action:** Send HTML email to `hoshenyehuda@gmail.com`
- **Subject:** `ליד חדש – סדנת אשת חיל: {{first_name}} {{last_name}}`

### API Proxy Service
- **Location:** `/opt/merkazneshama/eshet-chayil-api/`
- **Endpoint:** `POST /api/lead`
- **Port:** `3444` (local only, proxied via Caddy)
- **Systemd:** `eshet-chayil-api.service`

---

## Environment Variables (`.env`)

```
PORT=3444
NODE_ENV=production
DIRECTUS_URL=http://127.0.0.1:18055
DIRECTUS_ADMIN_EMAIL=admin@crm.coriathost.cloud
DIRECTUS_ADMIN_PASSWORD=<stored securely>
DIRECTUS_COLLECTION=eshet_chayil_leads
RATE_LIMIT_WINDOW_SEC=60
RATE_LIMIT_MAX=5
CORS_ORIGIN=https://eshet-chayil.merkazneshama.co.il
```

---

## Security Measures

| Measure | Implementation |
|---------|----------------|
| No exposed tokens | Directus creds in server env only, never in client JS |
| Input validation | Required fields, email/phone format checks |
| Sanitization | HTML stripped, lengths truncated |
| Rate limiting | 5 requests/IP per 60 seconds |
| Honeypot | Hidden `website` field; filled = silent reject |
| CORS | Only allows landing-page origin |
| Systemd hardening | NoNewPrivileges, ProtectSystem, MemoryMax |

---

## Testing

### Curl test (local):
```bash
curl -s http://127.0.0.1:3444/health
curl -s -X POST http://127.0.0.1:3444/api/lead \
  -H 'Content-Type: application/json' \
  -d '{"first_name":"Test","last_name":"User","phone":"0501234567","email":"test@example.com","consent":true}'
```

### Curl test (public):
```bash
curl -s -X POST https://eshet-chayil.merkazneshama.co.il/api/lead \
  -H 'Content-Type: application/json' \
  -H 'Origin: https://eshet-chayil.merkazneshama.co.il' \
  -d '{"first_name":"Test","last_name":"User","phone":"0501234567","email":"test@example.com","consent":true}'
```

Expected response: `{"ok":true}`

---

## Checklist

- [ ] Form submission creates Directus item
- [ ] Email received at `hoshenyehuda@gmail.com`
- [ ] WhatsApp button opens `wa.me/972543200050` with prefilled message
- [ ] Rate limiting blocks >5 requests/min from same IP
- [ ] Honeypot silently rejects bot submissions

---

## Maintenance

### Restart API:
```bash
sudo systemctl restart eshet-chayil-api
```

### View logs:
```bash
sudo journalctl -u eshet-chayil-api -f
```

### Check Directus leads:
1. Go to `https://crm.coriathost.cloud/admin`
2. Navigate to "לידים - אשת חיל" collection
