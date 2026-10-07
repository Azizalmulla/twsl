# OctoChat

OctoChat is AI Octopus's communication platform.

Backend owner: @Azizalmulla. Frontend owner: @M7sn08.

## Repository layout

- `backend/` — backend implementation owned by @Azizalmulla.
- `frontend/` — React Native frontend implementation owned by @M7sn08.
- `docs/PRD.md` — canonical product requirements, preserved as supplied.
- `docs/ARCHITECTURE.md`, `docs/DATA_MODEL.md`, `docs/SECURITY.md` — pending approved specifications.
- `shared/contracts/` — authoritative HTTP and realtime interface definitions, pending agreement.
- `AGENTS.md` — engineering instructions and ownership boundaries.
- `.github/CODEOWNERS` — review ownership.

## Before coding

Read `AGENTS.md`, all files under `docs/`, and everything under `shared/contracts/`.
Architecture, data, security, and shared contracts are being finalized before feature implementation begins.
Shared contracts are authoritative; interface changes must be explicit and reviewed by both owners.

The supplied PRD retains references to `mobile/`. This repository uses `frontend/` for the frontend work area, as explicitly agreed by the owners; the canonical PRD has not been rewritten.

## Development workflow

Never work directly on `main`. Backend and frontend work belongs on their respective branches or focused feature branches; shared foundation work uses `chore/project-foundation`.
Changes enter `main` through focused, reviewable pull requests. Respect ownership boundaries and never commit secrets or production environment files.
