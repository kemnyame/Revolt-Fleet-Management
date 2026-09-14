# Revolt Corporate Fleet Management

Deployment-ready Node.js fleet-management application for Railway. The application includes the web UI, backend REST API, populated fleet data, SQLite persistence, GPS ingestion/history endpoints, and a health check.

## Railway deployment

1. Push the contents of this folder to a GitHub repository.
2. In Railway, create a **New Project > Deploy from GitHub Repo** and select the repository.
3. Add a **Volume** to the web service and mount it at `/data`.
4. In **Variables**, add:
   - `DATABASE_PATH=/data/revolt-fleet.db`
   - `NODE_ENV=production`
   - `ADMIN_EMAIL=<your admin email>`
   - `ADMIN_PASSWORD=<a strong password>`
5. Do not set `PORT`; Railway supplies it automatically.
6. Deploy. `railway.json` supplies `npm start` and the `/api/health` health check.
7. In **Networking**, generate a public domain or attach your custom domain.

On the first start with an empty volume, the application automatically creates and populates the database with demonstration fleet data.

## Local run

Requires Node.js 22+.

```bash
npm start
```

Open `http://localhost:3000`.

## GPS API

Health check:

`GET /api/health`

GPS ingestion:

`POST /api/gps/ingest`

Example JSON body:

```json
{
  "tracker_id": "GPS-1000",
  "lat": 5.6037,
  "lng": -0.1870,
  "speed": 42
}
```

GPS history:

`GET /api/gps/history?vehicle=RVT-102`

## Important production note

This package is deployment-ready for a hosted pilot/demo. Before storing sensitive real corporate fleet data, add production authentication/MFA, strict multi-tenant authorization, secret management, object storage, backups, and migrate high-volume telematics workloads to PostgreSQL or a dedicated telemetry store.

## UI Refresh - September 2026
This package includes the refreshed Revolt Fleet Intelligence interface: brighter corporate command-centre styling, vivid gradient page headers, redesigned navigation, KPI cards, tables, forms, modal windows and responsive mobile layouts. Existing API/database behaviour is retained.

## QA status
Validated locally: JavaScript syntax, Node server startup, health endpoint, bootstrap data, resource GET endpoints, vehicle/driver/fuel/incident creation, GPS ingestion and GPS history. Some demonstration-only controls remain intentionally non-persistent: geofence drawing, generic trip/maintenance/document/user forms, report generation, alert acknowledgement and settings save. These require the next workflow/API implementation layer before production customer use.

## Full functional build update
This release adds database-backed CRUD for vehicles, drivers, trips, maintenance, fuel, incidents, documents, users and geofences; persistent alerts and settings; secure password hashing and token sessions; tenant-scoped data; audit logs; GPS history; and a live GPS simulator that moves connected demo vehicles around their current Accra coordinates. The simulator is for demonstrations and testing. It is not a substitute for physical tracker telemetry.

Railway variables:
- `DATABASE_PATH=/data/revolt-fleet.db`
- `ADMIN_EMAIL=your-admin-email`
- `ADMIN_PASSWORD=your-strong-password`
- `NODE_ENV=production`

Keep the Railway persistent volume mounted at `/data`.
