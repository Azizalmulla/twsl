# OctoChat Engineering Instructions

OctoChat is a production-oriented realtime messaging and business communication platform.

Before modifying implementation, read:
- docs/PRD.md
- docs/ARCHITECTURE.md
- docs/DATA_MODEL.md
- docs/SECURITY.md
- shared/contracts/

Ownership:

/backend
Primary owner: @Azizalmulla

/frontend
Primary owner: @M7sn08

/docs
Shared ownership

/shared/contracts
Shared ownership

Rules:

1. Never work directly on main.
2. Backend and frontend work must happen on their respective branches or focused feature branches.
3. Never silently change shared APIs, event schemas, product behavior, or persisted data semantics.
4. Frontend must never invent undocumented backend endpoints.
5. Backend must never silently change an interface relied on by frontend.
6. Shared contract changes must be explicit and reviewed.
7. Do not implement custom cryptographic algorithms or protocols.
8. Never commit API keys, passwords, OTP credentials, signing keys, certificates, secrets, private keys, or production environment files.
9. Treat reliability, security, observability, maintainability, performance, and Arabic/RTL support as first-class requirements.
10. Do not build features outside approved product scope.
11. Do not introduce unnecessary distributed infrastructure for prestige.
12. Add tests for important behavior.
13. Pull requests should be focused and reviewable.
14. Code must not contradict canonical documentation/contracts without explicitly updating them first.

Locked technology direction:

Backend:
- Go
- PostgreSQL
- Redis where justified
- REST/HTTP for authoritative operations and synchronization
- WebSockets for realtime events
- S3-compatible object storage for media
- APNs and FCM orchestration for push

Frontend:
- React Native
- TypeScript
- SQLite local persistence
- iOS + Android

Contracts:
- OpenAPI 3.1
- versioned realtime event schemas

Architecture principles:
- PostgreSQL is authoritative.
- WebSockets are for realtime delivery, not the sole source of truth.
- clients recover through authoritative synchronization/history APIs.
- outgoing logical messages use client-generated client_message_id values.
- retries must be idempotent.
- server controls canonical message ordering.
- offline/reconnect behavior is a first-class requirement.
- users/devices are distinct concepts.
- media is not transported as large WebSocket payloads.
- no microservices/Kafka/Kubernetes unless actual scale requirements justify them.
