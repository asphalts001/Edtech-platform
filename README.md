# Asphalts EdTech Platform

A complete digitalization infrastructure for coaching institutes — built to handle the full operational loop of a acadmic preparation environment. Three components, one coherent system.

---

## What This Is

Most EdTech tools solve one problem. This system solves the whole chain — from a student sitting a test on paper, to a teacher scanning the answer sheet in a staffroom with spotty WiFi, to a score appearing on a parent's phone, to an admin reviewing batch performance across weeks of tests.

Every component was designed around the actual constraints of the environment: budget Android devices, unreliable connectivity, no tolerance for per-scan cloud costs, and teachers who need results now.

---
## Phase 1 - complited

## The Three Components

### Student App
[`asphalts001/student-app`](https://github.com/asphalts001/student-app)

Android application for students. Receives targeted study resources, displays test results, and stays synced through unreliable connectivity via CoreSync. Built to survive process death on budget 2GB devices — WorkManager ensures a result persists through reboots and network drops.


### API Backend
[`asphalts001/legacy-backendApi`](https://github.com/asphalts001/legacy-backendApi)

Production API running on AWS. Handles auth, student and batch management, test administration, resource delivery, score ingestion, and SMS notifications.

```
Runtime:    Node.js v22 + TypeScript
Framework:  Express 5
Database:   RDS PostgreSQL 16, private subnet
Cache:      Redis (OTP, 10-min expiry)
Storage:    S3
Email:      AWS SES
Process:    PM2 on Ubuntu EC2
SSL:        Let's Encrypt via Nginx
```

The job queue runs as a separate worker process using `FOR UPDATE SKIP LOCKED` — multiple workers can run without stepping on each other. Idempotency keys are enforced at the database level, so Android retries never produce duplicate records. UUIDs are generated on-device so offline clients never need a server roundtrip to create a record.

Live at `https://api.theasphalts.com`

### CoreSync AAR
[`asphalts001/coresync`](https://github.com/asphalts001/coresync) — private library

An Android library shared across both apps. Queues sync events to Room, fires WorkManager when connectivity arrives, pushes to the API with exponential backoff and deduplication, and clears synced rows on confirmation.

Integration is three method calls. Everything else is invisible.

```kotlin
CoreSync.init(context = this, apiBaseUrl = "https://api.theasphalts.com", token = token)
CoreSync.enqueueTestEvent(studentId, testId, marksObtained, deviceId)
CoreSync.acknowledgeResource(pushId)
```

---

## Infrastructure

| What | Where |
|---|---|
| API | `https://api.theasphalts.com` |
| Cloud | AWS ap-southeast-2 |
| Database | RDS PostgreSQL 16, private subnet |
| Storage | S3 |
| Cache | Redis on EC2 |
| Email | AWS SES |
| SSL | Let's Encrypt via Nginx |

Running at roughly $60/month. Lean, live, with clear upgrade paths at each bottleneck.

---

## Phase 2 Roadmap

**Admin terminal** — specified in the system blueprint. Batch management, test oversight, resource publishing, and performance dashboards for institute administrators.

**Institute website** — Next.js, designed for public-facing discovery and student onboarding. The institute's front door online.

**OMR Scanner** -
Built into the institute's operational workflow.

A teacher points the app at a printed answer sheet. OpenCV detects the sheet boundary, corrects perspective, reads bubbles using adaptive thresholding, decodes the student and test identity from a QR code via ZXing — architected for fully offline edge execution, transitioning from a lean cloud-ingestion pipeline to an optimized local processing model to completely bypass cloud vision API costs. Score is calculated on-device, sheet image uploads to S3, result queues for SMS delivery to student and parent.

---

## Also In This Profile

**[Legacy](https://github.com/asphalts001/legacy)** — a separate standalone study app built independently. Cordova-packaged, vanilla JavaScript frontend, Supabase backend, offline-first sync, 789 bundled MCQ questions. A different scope, the same thinking.

**[Legacy Messenger](https://github.com/asphalts001/legacy-messenger)** — an early-stage messenger architecture built around circles and pools rather than open contact lists. Client-side E2E encryption, no ads, subscription revenue model. Primitive version initiated — two users, a circle, terminal-based conversation. Architecture documented.

---

*This repository contains documentation only. Component code lives in the linked repositories above.*

---
