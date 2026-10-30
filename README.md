# VitaPulse — Smart Fitness & Wellness Platform

VitaPulse is a polished fitness tracking application built with **Angular 20** and **ASP.NET Core 10 Web API**. It combines account-based workout tracking with activity insights and a responsive dashboard.

## What's new in this edition
- Rebranded product identity and refreshed responsive UI.
- Weekly activity chart with a 150-minute movement goal and progress indicator.
- Consecutive-day activity streak based on completed workouts.
- Workout catalogue search and category filters (cardio, strength, recovery and low impact).
- Live workout timer, progress ring, estimated calories and completion history.
- JWT authentication, BCrypt password hashing, server-validated sessions, user-scoped workout history and SQLite persistence.
- Empty states, inline API feedback, mobile layouts and keyboard-friendly search controls.

## Requirements
- Node.js 20.19+ or 22.12+ and npm
- .NET 10 SDK

## Project layout
- `vitapulse-angular20/` — Angular 20 frontend
- `vitapulse-dotnet/` — ASP.NET Core 10 Web API, JWT authentication, EF Core 10 and SQLite
- `start-frontend.ps1` / `start-backend.ps1` — optional Windows PowerShell launch scripts

## Run locally
Open two PowerShell terminals from the extracted project root.

**Terminal 1 — API**
```powershell
cd .\vitapulse-dotnet
dotnet restore
dotnet build
dotnet run
```
API: http://localhost:5080 · Swagger: http://localhost:5080/swagger · Health: http://localhost:5080/health

**Terminal 2 — frontend**
```powershell
cd .\vitapulse-angular20
npm install
npm start
```
Open http://localhost:4200, create an account, and keep the API running. Starter exercises are seeded automatically.

## API routes
- `POST /api/auth/register` — register
- `POST /api/auth/login` — sign in and receive a JWT
- `GET /api/auth/me` — current account (JWT required)
- `GET /api/exercises` — available exercises (JWT required)
- `GET /api/workouts` — current user's workout history (JWT required)
- `POST /api/workouts` — save a completed or early-finished session (JWT required)
- `DELETE /api/workouts/{id}` — delete a workout owned by the current user (JWT required)

## Important notes
- This migration uses a .NET API and SQLite rather than the original Firebase setup. Firebase configuration is not required.
- The JWT signing key in `vitapulse-dotnet/appsettings.json` is for local development only. Replace it with a secure secret before deployment.
- The activity goal is a motivational in-app target, not medical advice. Calories are estimates.
- To reset local data, stop the API and delete `vitapulse.db` in the backend folder; it will be recreated on startup.

## Added wellness features

- **Goals & Wellness** — set weekly workout-session and active-minute goals; progress is calculated from completed workouts.
- **Hydration Tracker** — record daily glasses, configure glass size and daily target, and review a seven-day hydration log. Hydration entries/settings are stored in browser local storage per signed-in email.
- **Weekly Consistency** — review completed sessions, workout minutes, calories, and active days for Monday through Sunday.
- **Navigation** — dedicated Goals & Wellness, Hydration Tracker, and Weekly Consistency tabs are available after sign-in.
- **Authentication** — existing .NET API registration/login uses JWT bearer tokens and BCrypt password hashing; protected application pages use the auth guard.
- **Responsive UI** — new pages use responsive layouts for desktop and mobile.

The hydration tracker is a habit log, not medical advice. The wellness targets are editable and should be personalized.


## Repair notes (Angular 20 + .NET 10)
- Backend targets `net10.0` and uses EF Core / JWT bearer packages 10.0.0.
- The SQLite file path is resolved relative to the backend project directory, avoiding accidental database changes when launched from a different working directory.
- Protected routes validate the stored JWT against `GET /api/auth/me`. A stale token is cleared and the user is sent back to sign in.
- Workout endpoints check that both the signed-in user and selected exercise exist before saving, so stale sessions return a useful 401/400 instead of a SQLite foreign-key 500.
- Run `dotnet restore` and `dotnet build` in `vitapulse-dotnet`; run `npm install` and `npm run build` in `vitapulse-angular20`. This source archive has not been compiled in this environment because package installation did not complete here.


## Final UI themes
The Angular application now includes only Light and Dark appearance modes, controlled by a toggle slider. The choice is persisted in browser local storage. See `FINAL_PROJECT_GUIDE.md` for final setup and verification instructions.


## Advanced 3D Motion Lab
The Angular application includes a Three.js-powered 3D trainer beside the active workout timer and in the floating workout card. Its procedural avatar uses different motion profiles for running, walking, cycling, squats, push-ups, and one combined Yoga & Stretching activity. The avatar is stylized procedural geometry, not a photorealistic asset. Separate legacy Yoga and Stretching options are hidden from the selector without deleting old database rows that may be referenced by workout history.
