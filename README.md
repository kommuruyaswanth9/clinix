# ClinixFlow — live care-flow prototype

This care-flow application implements the supplied two-pathway workflow: an arrival-ordered Routine OPD queue and an independent Emergency pathway.

## Start on Windows PowerShell

1. Download or clone the ClinixFlow project.
2. Open PowerShell in the ClinixFlow project folder.
3. Run:

```powershell
npm.cmd run seed
npm.cmd start
```

4. Open the local address printed by PowerShell. The app starts at port 3000 and tries the following ten ports when 3000 is busy.

No `npm install` is required. This project uses built-in Node.js modules. Use Node.js 18 or newer.

## Included improvements

- Live local date and time in the header, updated every second.
- Background data refresh every 20 seconds without rebuilding an in-progress form, so typed registration, delay, search, and edit data stays visible.
- Manual refresh button for an immediate dashboard update.
- Edit button on routine and emergency records. It preserves token, registration ID, pathway, status, and activity history.
- Symptom note prompts that help staff capture reported symptoms. They do not diagnose or recommend care.
- Updated modern dashboard styling, improved action controls, and responsive layout.
- ETA is shown as a real-time minutes-and-seconds countdown and follows the same overall routine queue order shown in the table.
- Patient phone number field with a Call action for waiting routine patients.
- Calendar-based reports for routine registrations, routine completions, emergency registrations, and emergency completions.
- The overall landing page is named Operations dashboard; there is no separate operational-metrics page.
- Separate Token history and Patient history tools: Token history uses an R/E token number; Patient history uses the phone number and returns all recorded visits for that number.

## Workflow implemented

1. Staff registers the patient and selects **Routine OPD** or **Emergency**.
2. Routine creates an R-series token; Emergency creates an E-series tracking token.
3. Routine assigns every active R-series token one unique overall position, with an approximate ETA and status flow: `Waiting → In consultation → Completed`.
4. Emergency uses a separate status flow: `Awaiting assessment → In emergency care → Awaiting further care → Completed`.
5. Emergency records never enter or reorder the routine queue.
6. Staff can record an operational delay that changes ETA only for the affected routine department.
7. Token history uses an R-series or E-series token number. Patient history uses a phone number and returns all matching recorded visits with symptoms, vitals, and status events.

## Safety limits

This is not a medical device. Staff decide the care pathway; the app does not diagnose patients or decide treatment priority. Follow local hospital emergency protocol for any possible emergency. Do not enter real patient data.

## Verify and reset

```powershell
npm.cmd test
npm.cmd run build
npm.cmd run seed
```

Data is stored locally in `data/clinixflow.json` when using the packaged release.
## Public deployment with Render

GitHub Pages cannot run this Node.js application because it only hosts static files. To publish ClinixFlow publicly:

1. Open https://dashboard.render.com/ and sign in with GitHub.
2. Select **New → Web Service**, then select the `ClinixFlow` repository.
3. Use branch `main`, build command `npm run build`, and start command `npm start`.
4. Click **Create Web Service**. Render provides a public HTTPS address ending in `.onrender.com`.

The app uses local JSON storage for the prototype. Data can reset when the service restarts or redeploys.
## Operations intelligence

ClinixFlow calculates a transparent ETA range from the active queue, patients ahead, a consultation-time assumption, and recorded department delays. The routine queue and reports provide flow visibility. When a staff member records an operational delay, affected routine patients receive an in-app ETA-change notification. These features analyse flow operations only; they never diagnose a patient or determine clinical treatment priority.
## Dynamic consultation timing and calendar reports

Each patient receives a stored consultation duration from 5 to 15 minutes. ETAs use those patient-specific durations and update as the active consultation progresses. Selecting a report date switches to the daily report automatically; the starter dataset contains recent completed records so the calendar can demonstrate distinct daily totals.
## Email-ready consultation notification

Registration records a patient email address. When staff completes a routine consultation, ClinixFlow identifies the next waiting routine patient and prepares a ready-for-consultation email. Selecting the email action opens the staff device’s configured email client with the message prefilled. Sending the message remains a staff action.
## Compact ETA and email content

Routine consultation durations are assigned per patient from 5 to 10 minutes. The ready-for-consultation email includes the patient’s current ETA and estimated consultation range.
## Brevo email OTP login

The home page is a Gmail OTP login page. Copy .env.example to .env, then set GMAIL_USER and a Google App Password in GMAIL_APP_PASSWORD. OTPs expire after two minutes and allow five verification attempts. Do not commit .env.
