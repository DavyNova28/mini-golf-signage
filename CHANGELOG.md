# Changelog

All notable changes to the **Mini Golf Signage / Dashboard** project are documented here.

> **Versioning note:** The project used more than one versioning scheme during its early development.  
> The **current Dashboard release line** is tied to numbered builds (for example, v1.4.0 / Build 117.3 and v1.5.0 / Build 118).  
> Older July 2026 entries are preserved under **Legacy Project History** exactly as historical milestones, so some version numbers may repeat.

---

# Current Dashboard Release Line

## [v1.5.0] - 2026-09-14 — Build 118

**Holiday Source Dropdowns**

This release improves the **Holiday Schedule Manager** by replacing manual source-tab entry with controlled, screen-specific dropdown menus. The goal is to reduce typing mistakes while preserving the existing Holiday Schedule workflow and routing behavior.

### Added

- Replaced the Arcade, Golf, Slush, and infoArcade Holiday source-tab text fields with controlled dropdown menus.
- Added screen-specific source allowlists so each screen can only select approved schedule tabs for that screen.
- Grouped available source tabs by schedule type where applicable:
  - Holiday
  - Regular
  - Summer
  - Promo
- New Holiday Schedule Days continue to default to the standard reusable Holiday tabs:
  - `ArcadeHoliday`
  - `GolfHoliday`
  - `SlushHoliday`
  - `infoArcadeHoliday`

### Improved

- Reduced the risk of misspelled or invalid source-tab names when creating or editing a Holiday Schedule Day.
- Existing approved source selections automatically repopulate in the new dropdown controls.
- CLOSED days continue to show **Not Used - Closed** while preserving the selected source values underneath.
- Unchecking CLOSED restores the previously selected source tabs.
- Existing legacy or unapproved values are shown as **Current / unapproved** instead of being silently replaced.
- Enabled, open Holiday Schedule Days cannot be saved while an unapproved source remains selected.

### Unchanged

- Holiday Schedule routing priority remains unchanged.
- Holiday Calendar integration remains unchanged.
- Duplicate-date protection remains unchanged.
- Audit logging and Audit Log Retention remain unchanged.
- Offline fallback and Profile-Aware Routing behavior remain unchanged.
- No Google Apps Script changes or redeployment are required. DEV and PROD continue to use the shared Apps Script backend.

### Release Information

- **Version:** v1.5.0
- **Build:** 118
- **Channel:** Stable
- **Status:** Stable Release

---

## [v1.4.0] - 2026-09-13 — Build 117.3

**Holiday Scheduling & Reliability**

This release promotes the Dashboard from **v1.3.0 / Build 113** to **v1.4.0 / Build 117.3** and focuses on safer special-day scheduling, clearer routing visibility, audit-log housekeeping, and release hardening.

## Highlights

- Added a full **Holiday Schedule Days** system for date-specific opening hours, closures, and per-screen source tabs.
- Added the **Holiday Schedule Manager** directly in Dashboard V3.
- Integrated Holiday Schedule Days into the **Holiday Calendar** and Profile-Aware Routing.
- Added optional **Recurring Promo Days** routing rules.
- Added configurable **Audit Log Retention** with automatic cleanup.
- Hardened cache/fallback behavior, save error handling, duplicate-date validation, and refresh lifecycle behavior.

## Holiday Schedule Days

- Added the new `Holiday Schedule Days` configuration model for one-off special operating dates.
- Each date can define:
  - label
  - opening and closing time
  - Arcade source tab
  - Golf source tab
  - Slush source tab
  - infoArcade source tab
  - Closed state
  - Enabled state
- Enabled and valid Holiday Schedule Days take priority over legacy Holiday Overrides, recurring Promo Rules, and normal profile routing.
- Closed special days do not require opening/closing times or reusable source tabs.
- Added reusable default Holiday source tabs such as `ArcadeHoliday`, `GolfHoliday`, `SlushHoliday`, and `infoArcadeHoliday`.
- Added validation for missing dates, invalid hours, missing source tabs, and duplicate calendar dates.
- Added server-side duplicate-date validation as a second safety layer.

## Holiday Schedule Manager

- Added **Operations → Holiday Schedule Manager**.
- Added Add, Reload, Save, Delete, Closed, and Enabled controls.
- Existing rows now repopulate correctly after a full Dashboard reload.
- Disabled Holiday Schedule rows remain visible in the manager while staying excluded from live routing.
- Add and Save remain blocked until the Holiday Schedule Days feed has loaded successfully, preventing an empty/stale manager from overwriting existing Google Sheets data.
- Added clearer visual grouping:
  - Special Day
  - Business Hours
  - Schedule Tabs
  - Status
  - Actions
- Added OPEN DAY, CLOSED DAY, and DISABLED row badges.
- Reusable tab names now appear in distinct rounded fields for better readability.
- When Closed is checked, source-tab fields visibly show **Not Used - Closed** while preserving their underlying values.

## Holiday Calendar Integration

- Holiday Calendar now reads **Holiday Schedule Days** instead of relying on legacy Holiday Overrides for its primary special-day display.
- Calendar entries can show:
  - special-day label and hours
  - CLOSED
  - DISABLED
  - INVALID
- Disabled special days remain visible in a muted style.
- Calendar details now include Enabled/Disabled state, validation status, and source tabs.
- Calendar Reload refreshes Holiday Schedule Days.
- Add/Edit actions open the Holiday Schedule Manager directly.
- Adding from a selected date pre-fills that date and the standard Holiday source-tab defaults.
- Existing dates open for editing instead of creating another row for the same date.
- Legacy Holiday Overrides remain available for compatibility/fallback use.

## Recurring Promo Days

- Added an optional **Recurring Promo Days** manager.
- Promo Rules can route a selected profile/screen to a different source tab on a configured weekday.
- Default examples include:
  - Golf Wednesday → `GolfPromoWednesday`
  - Arcade Thursday → `ArcadePromoThursday`
- Promo Rules start disabled by default.
- Holiday scheduling continues to take priority over Promo Rules.
- Added Dashboard PIN-protected saving and routing-cache refresh after changes.

## Audit Log Retention

- Added configurable Audit Log retention:
  - 30 days
  - 60 days
  - 90 days (default)
  - Forever
- Added **Clear old logs now** to immediately remove entries older than the selected retention period.
- Retention changes and cleanup require the existing Dashboard save PIN.
- Automatic retention cleanup now runs safely after Dashboard write actions.
- Added Audit Log action labels/filters for Holiday Schedule saves, Promo Rules, retention changes, and cleanup.
- Added `Holiday Schedule Days` and `Audit Log` to destination filtering.
- Corrected Holiday Schedule audit row-count reporting.

## Notification Center Stability

- Hardened Notification Center interactions during background Dashboard refreshes.
- Open notification panels are no longer unnecessarily rebuilt while the user is interacting with them.
- Added keyed card updates so unrelated notification changes do not destroy hovered/active controls.
- Improved Open/Snooze click handling and duplicate-click suppression.
- Reduced hover flicker and lost interactions caused by live telemetry rerenders.

## Reliability and Safety

- CacheService is now treated as an optimization only; cache-write failures no longer break otherwise valid signage responses.
- Increased schedule-feed cache lifetime and added separate normalized configuration caching for slower-changing routing configuration.
- Added safe cache-size handling before writing serialized data.
- Holiday Schedule saves now return the correct error-response type when rejected, preventing the UI from remaining stuck on **Saving…**.
- Audit Retention actions now return the correct error-response type on failure instead of remaining stuck on Saving/Clearing states.
- Added client-side and server-side Holiday duplicate-date protection.
- Release/cache build markers were aligned with the active build to reduce stale JS/CSS reuse.
- Release Notes now use actual version/channel metadata instead of always presenting the build as a stable release.

## Offline / Fallback Hardening

- Cached screen snapshots now preserve Profile-Aware Routing metadata, including logical screen, profile, route key, source tab, label, and legacy request information.
- When live schedule data is temporarily unavailable, cached schedules can retain the real routing source instead of showing **Unknown source**.
- Promo/Holiday/Regular source type is retained in cached mode.
- The main refresh indicator now waits until all four schedule requests have settled.
- A request is considered settled whether it loads live, restores from cache, or finishes with an error.
- Fixed the endless refresh spinner when cached fallback succeeds.
- One fast screen can no longer stop the refresh indicator while the other screens are still loading.
- Timed-out/failed JSONP elements are cleaned up and stale responses from an older refresh generation are ignored.

## Build 117.x Corrective Fixes

- Fixed the Holiday Schedule save routing regression introduced during the first Build 117 Audit Retention integration.
- Fixed Holiday Schedule audit row-count parsing.
- Improved reusable source-tab field contrast in the Holiday Schedule Manager.
- Hardened Holiday Schedule and Audit Retention failure paths before release.

## Upgrade Notes

- Dashboard release target: **Build 117.3**.
- The production Dashboard should use the same tested HTML/CSS/JS behavior as the final DEV release candidate.
- DEV and PROD intentionally share the same Apps Script backend in this deployment, so no separate production Apps Script instance is required.

---

## [v1.3.0] - 2026-08-16 — Build 113

**Complete Summer Schedule Routing**

### Added

- Completed profile-aware Summer routing for all four signage screens.
- Added dedicated Sunday routing for Golf, Slush, and infoArcade.
- Added the following Sunday schedule tabs:
  - `GolfSunday`
  - `SlushSunday`
  - `infoArcadeSunday`
- Continued use of `ArcadeSunday` for the Arcade Sunday schedule.

### Routing

- **Arcade**
  - Monday–Thursday → `ArcadeWeek`
  - Friday–Saturday → `Arcade`
  - Sunday → `ArcadeSunday`
- **Golf**
  - Monday–Saturday → `Golf`
  - Sunday → `GolfSunday`
- **Slush**
  - Monday–Saturday → `Slush`
  - Sunday → `SlushSunday`
- **infoArcade**
  - Monday–Saturday → `infoArcade`
  - Sunday → `infoArcadeSunday`

### Changed

- Removed the need for a manual Sunday schedule switch once the new tabs were configured.
- Continued the move toward a profile-aware Operations Center that understands seasonal business schedules.

---

## [v1.2 Stable] — Builds 89.5–90.4

### Build 89.5

- Reliable rollout interactions.

### Build 89.6

- Stable rollout player URLs.

### Build 89.7

- Render stability improvements.

### Build 90.0

- Toast notifications.

### Build 90.1

- Live heartbeat cards.

### Build 90.2

- Deployment history.

### Build 90.3

- Bulk rollout actions.

### Build 90.4

- Animated field updates.

---

# Legacy Project History

These entries are preserved from the original July 2026 project changelog. They predate the later Build-based Dashboard release line above.

## [3.0.0] - 2026-07-27

Official release of Dashboard V3 with a large set of new features.

### Added

- Schedule Manager fully operational.
- Daily Schedule.
- Holiday Overrides.
- Backup History.
- Image Library.
- Audit Log.
- Holiday Calendar.
- System Health.

---

## [2.0.0] - 2026-07-26

### Added

- Dashboard V2 control center.
- Larger real-time scheduled image previews.
- Live countdowns to the next change.
- Twenty-four-hour schedule timelines.
- Current-time markers on each timeline.
- Global next-change summary.
- Missing-image detection.
- System health reporting.
- Responsive desktop, tablet, and mobile layouts.

### Changed

- Dashboard schedule data refreshes every 30 seconds.
- Countdown timers and timeline markers update locally every second.

---

## [1.5.0] - 2026-07-26

### Added

- Web-based signage control dashboard.
- Real-time scheduled image previews.
- Regular and holiday schedule status indicators.
- Current and next image information.
- Full-screen and debug-view shortcuts.
- Automatic dashboard refresh every 30 seconds.

---

## [1.4.0] - 2026-07-26

### Changed

- Improved reading of the start time in the `Holiday Overrides` tab.

---

## [1.3.0] - 2026-07-26

### Changed

- Improved image transitions to keep the previous image visible underneath.
- Disabled periodic OptiSigns webpage refresh to prevent black-screen flashes.
- Added cache-version support to the OptiSigns URL.

### Fixed

- Fixed brief black flashes during scheduled image transitions.
- Fixed multi-screen URLs displaying a black screen.

---

## [1.2.0] - 2026-07-26

### Added

- Support for multiple screens using Google Sheet tab names.
- Support for URLs such as `?screen=Arcade`.
- On-screen debugging with `&debug=1`.
- Better error messages for missing images and invalid schedules.

---

## [1.1.0] - 2026-07-26

### Added

- Google Sheets schedule integration.
- Smooth image fading.
- Automatic image preloading.
- GitHub Pages hosting.
- Google Apps Script schedule feed.

---

## [1.0.0] - 2026-07-26

### Added

- Initial Mini Golf Signage website.
- Two-image scheduled display.
- OptiSigns Website asset support.

---

# Historical Unreleased Snapshot

The following items appeared under **Unreleased / Planned** in the original changelog. They are preserved here as historical notes and are **not automatically the current roadmap**.

- Day-of-week schedules.
- Automatic recovery health check.
