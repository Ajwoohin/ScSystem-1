SCSystem

A gamified campus engagement platform designed to transform school environments,
inspired by the point-based social structure of Classroom of the Elite.

Status: Active Development (Capstone Project) — Private Beta
Platform: Android (APK sideload)
Author: Solo-developed by a BSIS student, with AI-assisted development

THE IDEA

Many educational institutions have students who work on campus, and schools often face
staffing challenges in areas like facility upkeep and student support services. SCSystem
turns everyday campus participation into a rewarding, gamified experience — while giving
the school practical tools it currently lacks.

Students earn SCpoints by completing campus "quests," participating in events, and
interacting with each other and staff — turning civic participation into something visible,
trackable, and rewarding. Faculty and staff can post announcements, create quests, and
reward students directly — all backed by a tamper-resistant points ledger.

CORE FEATURES

- Institutional email + ID-based Sign Up / Log In — Working
- Light / Dark theme selection (persisted) — Working
- Student profile screen (name, ID, SCpoints, bio, QR) — Working
- QR code identity (per student) — Working
- Camera-based QR scanning ("Check Profile" / "Send Points") — Working
- Tamper-resistant SCpoints ledger (award_points, transfer_points) — Working
- Points History (auditable transaction log) — Working
- Quest board (database-backed, with acceptance limits) — Working
- Administrator role (real backing account, quest management) — Working
- Announcements / News (with "Club News" category tab) — Working
- Image attachments + auto-delete timer on announcements — Working
- Secret staff/teacher sign-up trigger + admin approval flow — In Progress
- Pull-to-refresh (Profile, Quests, Points History) — Working
- Inactive-account & old-log cleanup functions — Built
- Teacher/staff quest posting, with attribution shown — Planned
- Teacher/staff own SCpoints balance (for rewarding students) — Planned
- Admin "Distribute Points" (with distribution history) — Planned
- QR-based staff attendance tracking — Planned
- Job / skills board (student ↔ faculty gig matching) — Planned
- Row Level Security hardening (pre-deployment) — Planned

TECH STACK

Frontend: Flutter (Dart) — single codebase, compiles to Android APK
Backend: Supabase — Postgres database, Authentication, Storage (free tier)
Auth: Supabase Auth, restricted to valid institutional emails for students; staff use a gated
sign-up flow with admin approval
Camera/QR: mobile_scanner (scanning), qr_flutter (generation)
Local persistence: shared_preferences (theme choice, session flags)
Target distribution: Sideloaded APK (no app store costs)

KEY ARCHITECTURE DECISIONS

Supabase over Firebase: Firebase's Cloud Storage requires paid plans for some features
even on free-tier limits. Supabase offers a full Postgres database, authentication, and file
storage entirely on its free tier without requiring billing setup.

Points are never directly writable by clients. All point changes go through security
definer Postgres functions (award_points, transfer_points), with Row Level Security
blocking any direct table writes. Every change is logged in point_transactions, which
students/staff can read but never edit or delete.

Peer-to-peer point sending is a real transfer, not free creation — the sender's balance is
deducted, preventing infinite point generation via repeated scanning.

Admin uses a real Supabase Auth account (not just a client-side flag) so it can securely
call the same protected point-awarding functions as everyone else.

Full details in docs/ARCHITECTURE.md.

REPOSITORY STRUCTURE

scsystem/
├── docs/        # Architecture notes, schema design
├── showcase/    # Selected code samples (not the full working app)
├── README.md
└── CHANGELOG.md # Session-by-session build log

Note: This is a capstone project repository. The full working source lives in a local/private
development environment; this repository shares architecture documentation and representative
code samples rather than the complete application source code.

ACADEMIC CONTEXT

This project is being developed as a BSIS capstone project, with an eventual goal of
proposing it as a functional engagement platform for educational institutions. Full
documentation is being developed alongside the working prototype.
