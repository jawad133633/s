# Clinic + Pharmacy Management System

Offline-first Windows desktop starter for Noor Medical Clinic, following the Step 0 scope supplied by Raza.

## Stack decision

The brief asks for MERN while its Key Decisions section recommends embedded SQLite for an offline, single-PC product. This implementation keeps React and Node/Express, uses SQLite (`better-sqlite3`) instead of MongoDB for local persistence, and wraps the app with Electron. The desktop database is stored in Electron's user data folder; no separate database service is required.

## Run in development

```sh
npm install
npm run dev
```

Open a second terminal for Electron with `npm run electron` after Vite is running. `npm run build` creates the renderer bundle. `npm run dist` builds the Windows installer with electron-builder.

## Authentication and permissions

- On first launch, the app guides the owner through creating the first Admin account. There is no open public signup route; clinic staff accounts are created by an authenticated Admin under Settings → User management.
- User passwords are stored as salted scrypt hashes. Login creates a random, hashed, eight-hour session. Logout, password change and account deactivation revoke sessions. Sign-in attempts are throttled after repeated failures.
- Admin-issued recovery codes are shown once. Forgot password uses the local username and recovery code, so the feature works offline and does not depend on email. A successful reset rotates the recovery code.
- Existing API routes for patient registration, queue, and stock are guarded by role authorization. Admin, Doctor, Receptionist and Pharmacist screens are filtered by their role.

## Current starter scope

- Desktop dashboard shell with role-specific navigation for Admin, Doctor, Receptionist, and Pharmacist.
- Patient directory and registration flow, today's token queue, call-next interaction, doctor visit and prescription editor, billing, pharmacy stock, reports, appointment and lab entry screens, settings and backup entry points.
- Express API for first-run setup, login/logout/session check, admin-created staff signup, password change/recovery, and role-gated patients, queue, stock and daily collection routes.
- SQLite schema for users, patients, tokens, visits, prescriptions, bills, stock, stock transactions and settings.
- Basic Electron main process and local server startup.

Several dashboard records remain sample UI data, and some screens are still entry points rather than complete workflows. Remaining Step 0 work includes wiring every screen to SQLite, saving clinical visits and prescriptions, billing and pharmacy dispensing, patient history, report filters and own-patient scoping for Doctor reports, actual backup/restore, print templates (including packaged Urdu font), and clean-machine Windows installer verification. API authorization currently covers the implemented patient, queue, stock, and daily-collection endpoints; it must be extended alongside the remaining endpoints.

## Roles from the supplied brief

Admin manages users, registration, queue, clinical notes, billing, inventory, reports and backups. Doctor views the queue, writes visits and prescriptions, and reports on their own patients. Receptionist registers patients, manages tokens and billing. Pharmacist manages stock and views stock reports. API authorization must enforce these permissions before production use.
