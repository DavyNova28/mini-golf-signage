# Mini Golf Signage Manager

Version **1.3.0 Stable** — Build **113**

A web-based operations and digital signage management system for the Mini Golf screens, schedules, player health, business profiles, analytics, and automated schedule routing.

---

## Current Stable Release

- Version: `1.3.0`
- Build: `113`
- Channel: `stable`
- Git tag: `v1.3.0`

Version 1.3.0 was promoted from the validated Build 113 Release Candidate.

---

## Project Structure

```text
dashboard-v3.html

css/
  dashboard.css

js/
  dashboard.js

version.json
CHANGELOG.md
README.md
```

The production system also uses a Google Apps Script backend connected to the signage Google Sheets workbook.

---

## Deployment

Upload the frontend files while preserving the folder structure:

- `dashboard-v3.html`
- `css/dashboard.css`
- `js/dashboard.js`
- `version.json`

Do not flatten the `css` or `js` folders.

The dashboard uses the configured Google Apps Script `/exec` deployment URL in `js/dashboard.js`.

When Apps Script changes are included in a release, deploy a **new version of the existing Web App deployment** so the public `/exec` URL remains unchanged.

---

# Version 1.3 Highlights

## 🏢 Business Profiles

The system understands the operating season and business hours instead of relying on one permanent schedule.

Supported profiles include:

- Summer
- Regular
- Special / Holiday schedule foundation

Summer and Regular profiles automatically determine the appropriate operating hours based on the current date and day of the week.

---

## 🧭 Profile-Aware Schedule Routing

Version 1.3 introduces automatic schedule source routing.

The four permanent logical players are:

- Arcade
- Golf
- Slush
- infoArcade

Players no longer need to be manually changed when the operating profile or day changes.

The Apps Script backend determines the appropriate Google Sheets schedule source automatically.

### Summer Routing

```text
Arcade
Mon–Thu → ArcadeWeek
Fri–Sat → Arcade
Sunday  → ArcadeSunday

Golf
Mon–Sat → Golf
Sunday  → GolfSunday

Slush
Mon–Sat → Slush
Sunday  → SlushSunday

infoArcade
Mon–Sat → infoArcade
Sunday  → infoArcadeSunday
```

Regular Season routing is also selected automatically according to the active weekday.

This allows the signage system to continue operating correctly even when the Dashboard is not open.

---

## 🕒 Business Hours Awareness

The Dashboard understands whether the business is:

- Open
- Closed
- Before opening
- After closing

It can also determine the next expected opening period.

Expected-screen monitoring respects business hours so screens are not incorrectly penalized when the business is closed.

---

## 🖥️ Screen Intelligence

System Health now understands whether each logical screen is actually expected to be online.

This improves heartbeat monitoring and prevents unnecessary warnings outside operating hours.

The system tracks:

- Expected Today
- Expected Now
- Online / Offline state
- Heartbeat freshness
- Business-hours context

---

## ❤️ System Health

System Health was expanded significantly in Version 1.3.

It includes:

- Player heartbeat monitoring
- Expected-screen awareness
- Health Score
- Business-hours-aware warnings
- Maintenance awareness
- Go-Live Readiness
- Rollout tracking
- Operational status information

---

## 🛠️ Maintenance Mode

Individual logical players can be placed into Maintenance Mode.

Maintenance state is integrated with operational monitoring so planned maintenance does not create misleading health warnings.

---

## 🚦 Go-Live Preflight

The Go-Live Readiness system provides operational checks before deployment.

Preflight can evaluate the current system state and identify conditions that should be reviewed before a rollout.

---

## 🚀 Rollout Assistant

The Rollout Assistant helps track screen deployment and rollout progress.

Version 1.3 integrates rollout information with the wider System Health and operational monitoring system.

---

## 📊 Operational Analytics

Version 1.3 introduces Operational History and analytics.

Analytics include:

- Expected Player Uptime
- Per-screen Expected Samples
- Per-screen Online Samples
- Per-screen uptime percentage
- Health history
- Historical range filtering
- CSV export

Player uptime sampling is business-hours aware.

Screens are not penalized simply because they are offline when they are not expected to be operating.

Startup sampling also waits for a valid heartbeat snapshot to prevent false downtime during Dashboard initialization.

---

## 📋 Operations Snapshot

The Dashboard includes an Operations Snapshot providing a consolidated view of the current operating state.

This brings together business-profile, player, health, schedule, and operational information in one place.

---

## 📅 Daily Schedule Calendar

The Daily Schedule Calendar supports the four logical players:

```text
Arcade
Golf
Slush
infoArcade
```

The calendar provides a visual overview of the current signage schedules and adapts to the four-player architecture introduced in Version 1.3.

---

## 🗂️ Schedule Manager

Schedule Manager remains the central interface for reviewing and modifying signage schedules.

It works alongside Profile-Aware Schedule Routing so schedule content and automatic routing remain separate responsibilities.

---

# Automatic Operation

A major goal of Version 1.3 is reducing the amount of manual intervention required.

Once configured, the system automatically determines:

```text
Current date
      ↓
Business Profile
      ↓
Day of week
      ↓
Business Hours
      ↓
Correct Schedule Source
      ↓
Logical Player
      ↓
Store Screen
```

Season and weekday transitions therefore do not require the Dashboard to remain open.

---

# Deferred Real-World Validation

Version 1.3.0 has completed all validation that could be performed before the Regular Season transition.

Some calendar-dependent checks remain intentionally deferred:

### September

- Summer → Regular automatic transition
- Regular Monday / Tuesday closed behavior
- Regular Wednesday routing
- Regular Thursday / Friday routing
- Regular Saturday routing
- Regular Sunday routing

### Next June 23

- Regular → Summer automatic transition

These are **deferred validation items**, not known failures.

---

# Known Non-Blocking Item

The Daily Schedule Calendar **Jump to current time** control may not always visibly reposition the calendar as expected.

This does not affect:

- Schedule routing
- Store signage
- Google Sheets
- Player operation
- Business Profiles
- System Health
- Analytics

It is considered a non-blocking UI issue for future review.

---

# Stable Release Process

1. Validate changes in the Development environment.
2. Run the release/build checklist.
3. Verify System Health and Profile-Aware Schedule Routing.
4. Verify Schedule Manager and player heartbeats.
5. Validate Operational Analytics during business hours.
6. Promote the validated build to Production.
7. Run the Production smoke test.
8. Publish the corresponding GitHub release/tag.

---

# Release History

| Version | Build | Status |
|---|---:|---|
| `v1.3.0` | `113` | Stable |
| `v1.2.0` | Previous stable | Stable / rollback baseline |
| `v1.1.0` | `84` | Previous stable |
| `v1.0.0` | — | Original stable baseline |

---

## Version 1.1 Highlights

Version 1.1 introduced:

- Back to Top navigation and scroll progress
- Proactive Notification Center
- Notification read tracking and preferences
- Alert snoozing
- Notification History and history insights
- JSON history export
- Home layout personalization

---

# Production Release

**Mini Golf Signage Manager**

**Version 1.3.0 Stable**  
**Build 113**  
**Git tag: `v1.3.0`**

Validated through the Version 1.3 Release Candidate process and promoted to Production.
