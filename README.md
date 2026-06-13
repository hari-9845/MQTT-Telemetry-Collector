# MQTTR1 — MQTT Telemetry Collector (Full Stack)

Brief
- End-to-end telemetry collection, storage, simulation and visualization solution. Backend is ASP.NET Core with EF Core persisting to SQL Server / LocalDB; frontend is an Angular app with real‑time charts and a simulator. Includes JWT authentication, paging/DTOs, and background telemetry services.

Highlights
- Telemetry ingestion API (HTTP/MQTT-ready), EF Core-backed persistence
- Real-time Angular dashboard and simulator to generate test device data
- JWT-based auth with configurable settings in `appsettings.json`
- Background services: telemetry retention/cleanup and live telemetry generator
- Utilities: paging helpers, validators, repositories and DTOs
- Launch helpers: `START-ALL.bat` and `mqttr-frontend/setup-and-run.ps1`

Prerequisites
- .NET SDK (compatible with the backend project targets; workspace includes .NET 8 / .NET 10 projects)
- Node.js + npm (for the Angular frontend)
- SQL Server or LocalDB (DefaultConnection in `appsettings.json` uses LocalDB)
- PowerShell (preferred shell for provided scripts)

Quick start (recommended)
1. From repo root, run the launcher:
   - On Windows double-click or run: `START-ALL.bat`
   - This starts the backend API and the frontend dev server.
2. Backend
   - Backend will appear at: `http://localhost:5000`
   - Swagger UI: `http://localhost:5000/swagger`
   - Alternatively run manually:
     - `dotnet run --project MQTTR1\MQTTR1.csproj`
3. Frontend
   - From `mqttr-frontend` run:
     - `npm install` (first time)
     - `npm start` (dev server at `http://localhost:4200`)
   - Or use the helper script: `mqttr-frontend\setup-and-run.ps1`

Configuration (key files)
- `MQTTR1\appsettings.json`
  - `ConnectionStrings:DefaultConnection` — DB connection (LocalDB by default)
  - `TelemetrySettings` — `EnableDataCleanup` (bool), `RetentionDays` (int)
  - `JwtSettings` — `SecretKey`, `ExpirationHours`, `Issuer`, `Audience`
- `mqttr-frontend\src\environments\` — frontend environment endpoints

Database & seeding
- On first run the backend ensures DB is created and seeds a few example devices (see `Program.cs`).
- EF Migrations are present in `MQTTR1\Migrations`. The default connection targets LocalDB; change `DefaultConnection` to point to your SQL Server if needed.

Telemetry ingestion & simulation
- Use `ConsolePublisher` (sample JSON files included) to publish telemetry to the backend or to simulate MQTT payloads.
- The Angular simulator UI can generate device telemetry for testing and demos.

Useful commands
- Start both services: `START-ALL.bat`
- Start frontend only: run `mqttr-frontend\setup-and-run.ps1` then `npm start`
- Run backend: `dotnet run --project MQTTR1\MQTTR1.csproj`
- Apply EF migrations (manual):
  - `dotnet ef database update --project MQTTR1\MQTTR1.csproj --startup-project MQTTR1\MQTTR1.csproj`

Ports & endpoints
- Backend: `http://localhost:5000` (Swagger at `/swagger`)
- Frontend: `http://localhost:4200`

Project layout (selected)
- `MQTTR1/` — ASP.NET Core backend
  - `Controllers/` — API controllers (Telemetry, Auth, Devices, Users, Diagnostics)
  - `Data/ApplicationDbContext.cs` — EF Core DbContext
  - `Services/` — ingestion, auth, background hosted services
  - `Repositories/` — data access
  - `DTOs/`, `Models/`, `Utilities/`
- `mqttr-frontend/` — Angular SPA
  - `src/app/components/` — UI components (dashboard, simulator, telemetry, auth, management)
  - `services/` — API clients, simulator, auth handling
- `ConsolePublisher/` — small publisher used to send sample telemetry payloads

Security notes
- `JwtSettings:SecretKey` in `appsettings.json` is a placeholder. In production, store secrets in a secure store (Azure Key Vault, environment variables).
- Token validation in `Program.cs` is configurable — for production enable issuer/audience validation.

Developer tips
- Enable CORS in `Program.cs` has a permissive "AllowAll" policy for local dev; tighten for production.
- Response caching and memory caching are enabled to improve performance for frequently requested resources.
- The workspace includes helper scripts and a `START-ALL.bat` that opens separate windows for backend and frontend.

Files of interest
- `START-ALL.bat` — start both backend and frontend
- `mqttr-frontend/setup-and-run.ps1` — frontend install/start helper
- `ConsolePublisher/sample-single.json` and `sample-batch.json` — example telemetry payloads
- `MQTTR1/appsettings.json` and `appsettings.Development.json` — runtime configuration

Troubleshooting
- If frontend cannot reach backend, confirm proxy (`mqttr-frontend/proxy.conf.json`) and backend port (`5000`) match.
- If LocalDB is unavailable, update `DefaultConnection` to a reachable SQL Server instance.
- If Node/npm issues occur, run `mqttr-frontend\setup-and-run.ps1` which checks Node/npm and installs dependencies.

Contribution and development
- Follow existing code patterns: repository/service layers, DTOs, validators and paging helpers.
- Run unit tests (if present) using the Visual Studio Test Explorer or `dotnet test`.

License
- Project does not include an explicit license file. Add a license that fits your use case before publishing.

Contact / Next steps
- Use the simulator and ConsolePublisher to create telemetry, view it in the dashboard, and iterate on ingestion logic or UI visualizations.
- If you want, I can generate a condensed one‑paragraph summary for your resume or add a CONTRIBUTING.md.
