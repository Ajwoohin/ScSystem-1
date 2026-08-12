# SCsystem 🎓

A gamified campus engagement platform built for School System, inspired by the point-based social structure of *Classroom of the Elite*.

> **Status:** 🚧 Active Development (Capstone Project) — Private Beta
> **Platform:** Android (APK sideload)
> **Author:** Solo-developed by [Andrei Juan Gallemit], BSIS student, with AI-assisted development

---

## 💡 The Idea

Many students at SCCPAG are working students, and the college has faced staffing challenges in areas like facility upkeep and student support services. SCsystem turns everyday campus participation into a rewarding, gamified experience — while giving the school practical tools it currently lacks.

Students earn **SCpoints** by completing campus "quests," participating in events, and interacting with each other and staff — turning civic participation into something visible, trackable, and rewarding. Faculty and staff can post announcements, create quests, and reward students directly — all backed by a tamper-resistant points ledger.

## ✨ Core Features

| Feature | Status |
|---|---|
| Institutional email + EDP-based Sign Up / Log In | ✅ Working |
| Light / Dark theme selection (persisted) | ✅ Working |
| Student profile screen (name, EDP, SCpoints, bio, QR) | ✅ Working |
| QR code identity (per student) | ✅ Working |
| Camera-based QR scanning ("Check Profile" / "Send Points") | ✅ Working |
| Tamper-resistant SCpoints ledger (`award_points`, `transfer_points`) | ✅ Working |
| Points History (auditable transaction log) | ✅ Working |
| Quest board (database-backed, with acceptance limits) | ✅ Working |
| Administrator role (real backing account, quest management) | ✅ Working |
| Announcements / News (with "Club News" category tab) | ✅ Working |
| Image attachments + auto-delete timer on announcements | ✅ Working |
| Secret staff/teacher sign-up (`000` EDP trigger) + admin approval flow | 🚧 In Progress — known routing bug being fixed |
| Pull-to-refresh (Profile, Quests, Points History) | ✅ Working |
| Inactive-account & old-log cleanup functions | ✅ Built, not yet scheduled (safe manual testing only) |
| Teacher/staff quest posting, with attribution shown | 🔜 Planned |
| Teacher/staff own SCpoints balance (for rewarding students) | 🔜 Planned |
| Admin "Distribute Points" (with distribution history) | 🔜 Planned |
| QR-based staff attendance tracking | 🔜 Planned |
| Job / skills board (student ↔ faculty gig matching) | 🔜 Planned |
| Row Level Security hardening (pre-deployment) | 🔜 Planned |

## 🛠️ Tech Stack

- **Frontend:** Flutter (Dart) — single codebase, compiles to Android APK
- **Backend:** [Supabase](https://supabase.com) — Postgres database, Authentication, Storage (free tier)
- **Auth:** Supabase Auth, restricted to `@sccpag.edu.ph` institutional emails for students; staff use a gated sign-up flow with admin approval
- **Camera/QR:** `mobile_scanner` (scanning), `qr_flutter` (generation)
- **Local persistence:** `shared_preferences` (theme choice, session flags)
- **Target distribution:** Sideloaded APK (no Google Play Store costs)

## 📐 Key Architecture Decisions

- **Supabase over Firebase:** Firebase's Cloud Storage now requires the paid Blaze plan even for free-tier usage. Supabase offers a full Postgres database, authentication, and file storage entirely on its free tier with no credit card required.
- **Points are never directly writable by clients.** All point changes go through `security definer` Postgres functions (`award_points`, `transfer_points`), with Row Level Security blocking any direct table writes. Every change is logged in `point_transactions`, which students/staff can read but never edit or delete.
- **Peer-to-peer point sending is a real transfer, not free creation** — the sender's balance is deducted, preventing infinite point generation via repeated scanning.
- **Admin uses a real Supabase Auth account** (not just a client-side flag) so it can securely call the same protected point-awarding functions as everyone else.

Full details in [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md).

## 📁 Repository Structure

```
scsystem/
├── docs/               # Architecture notes, schema design
├── showcase/           # Selected code samples (not the full working app)
├── README.md
└── CHANGELOG.md        # Session-by-session build log
```

> **Note:** This is a capstone project repo. The full working source lives in a local/private development environment; this repository shares architecture documentation and representative code samples rather than the complete app source.

## 🎓 Academic Context

This project is being developed as a BSIS capstone project, with an eventual goal of proposing it as a real engagement platform for SCCPAG. Full documentation (Chapters 1–5) is being developed alongside the working prototype.

---

*Last updated: see [CHANGELOG.md](CHANGELOG.md) for the latest session notes.*
