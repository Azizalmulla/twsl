# OctoChat — Product Requirements Document

**Version:** 0.1  
**Status:** Initial canonical product specification  
**Repository:** `Azizalmulla/twsl`  
**Primary launch market:** Kuwait  
**Expansion intent:** GCC  
**Document owner:** OctoChat product/engineering team  
**Last updated:** 2026-10-06

> This file is the product source of truth for OctoChat.  
> `ARCHITECTURE.md`, the OpenAPI contract, realtime event schemas, and security design will define implementation details.  
> If code conflicts with this PRD, the conflict must be resolved explicitly rather than silently changing product behavior.

---

# 1. Product Summary

OctoChat is AI Octopus's independent consumer and business communication platform.

The product begins with a high-quality mobile messenger and a business-first discovery layer. Consumers can discover verified businesses, communicate with them, receive useful business activity, and message other OctoChat users. Businesses will later gain shared inboxes, AI agents, rich interactions, APIs, automations, bookings, commerce, payments, and other Octopus integrations.

OctoChat is **not** intended to be a visual clone of WhatsApp and is **not** merely a customer-support inbox.

The long-term objective is to own the communication rail connecting consumers, businesses, AI agents, and the wider Octopus ecosystem instead of depending on another messaging platform as the underlying network.

---

# 2. Product Thesis

OctoChat must solve two different problems at the same time:

## Consumer problem

A consumer needs a communication app that is:

- fast;
- trustworthy;
- private;
- easy to understand;
- useful even before all of their friends are using it;
- excellent in Arabic and English;
- capable of organizing business interactions better than a normal chat list.

## Business problem

A business needs:

- a direct communication channel with its customers;
- verified identity;
- lower dependency on third-party messaging infrastructure;
- flexible interactions;
- APIs and automation;
- human and AI support;
- future commerce, booking, payment, authentication, and operational integrations.

OctoChat's initial distribution wedge is **business-to-consumer communication**, while still building a genuine person-to-person messenger.

---

# 3. Product Goals

## 3.1 Primary goals

1. Build a reliable realtime messaging foundation owned by Octopus.
2. Deliver a premium iOS and Android consumer experience.
3. Support direct 1-to-1 user messaging.
4. Give users an immediate reason to open OctoChat through business discovery and verified business communication.
5. Make message delivery reliable across disconnects, retries, app restarts, weak networks, and offline periods.
6. Treat Arabic and English as first-class languages.
7. Build security and privacy into the architecture from the beginning.
8. Create clean frontend/backend contracts so multiple engineers and coding agents can work safely in parallel.
9. Design the foundation so business messaging, AI, bookings, payments, commerce, groups, calls, and GCC expansion can be added without rebuilding the messaging core.
10. Make the product feel fast and polished even during early releases.

## 3.2 What success looks like

A successful first serious OctoChat build allows two real users on two real devices to:

- create accounts;
- find each other;
- start a conversation;
- exchange messages instantly;
- go offline and reconnect;
- kill and reopen the app;
- retry sends safely;
- see correct message ordering;
- see sent/delivered/read states;
- receive push notifications;
- send media and voice notes;
- retain conversation history locally;
- use Arabic and English without broken layout.

It also allows the same consumer application to:

- discover verified businesses;
- view a business profile;
- start a business conversation;
- see useful business activity in a structured Activity surface.

---

# 4. Product Principles

## 4.1 Reliability before novelty

A message must not disappear, duplicate, reorder incorrectly, or show an incorrect state because the network changed or an app restarted.

Fancy features do not compensate for unreliable messaging.

## 4.2 Fast by default

Previously loaded content must render from local storage immediately where possible.

A chat list should not appear empty while waiting for a server request if the device already has cached data.

## 4.3 Business utility without marketplace clutter

Home helps users find and interact with businesses, but OctoChat must not feel like a banner-heavy advertising marketplace.

## 4.4 Consumer trust

Verification, privacy, predictable notifications, permission clarity, blocking, reporting, and spam prevention are product features, not backend chores.

## 4.5 Arabic is first-class

RTL layout, Arabic rendering, Arabic search, mixed Arabic/English messages, dates, numbers, punctuation, and input behavior must be tested throughout development.

## 4.6 No custom cryptography

OctoChat must never invent proprietary cryptographic algorithms or protocols.

Production E2EE must use an established, reviewed protocol/library selected through a dedicated security design.

## 4.7 One source of truth

Shared documentation and machine-readable contracts are authoritative.

Frontend and backend must not invent conflicting field names, states, endpoints, or event semantics.

## 4.8 Build for production, release in phases

"Production-grade" does not mean implementing every future feature immediately.

The system should have strong foundations while product scope is released progressively.

---

# 5. Target Users

## 5.1 Consumer

A person using OctoChat to:

- message another person;
- discover a business;
- contact a verified business;
- receive order, booking, support, delivery, or other business activity;
- keep business communication organized.

## 5.2 Business customer

A verified organization using OctoChat to:

- maintain an official identity;
- receive and send customer messages;
- eventually connect employees, AI agents, APIs, automations, commerce, bookings, and payments.

## 5.3 Business agent

An employee authorized to handle customer conversations on behalf of a business.

## 5.4 Developer

A developer integrating business software with the future OctoChat Business API.

---

# 6. Locked Technology Direction

The exact package/library versions will be chosen during architecture/setup work and kept current.

The following technology boundaries are locked unless changed through an explicit architecture decision.

## Mobile

- React Native
- TypeScript
- iOS and Android from one mobile codebase
- SQLite for local persistent application/message state
- APNs and FCM for push notification delivery

## Backend

- Go
- PostgreSQL as the authoritative relational datastore
- Redis for ephemeral/realtime state where appropriate
- WebSockets for realtime events
- REST/HTTP APIs for authoritative reads, writes, history, and synchronization
- S3-compatible object storage for media/files
- background job processing where required

## Contracts

- OpenAPI 3.1 will be the authoritative HTTP API contract.
- Realtime event schemas must be versioned and stored in the repository.
- Frontend types/clients should be generated from shared contracts where practical.
- Shared enums and states must not be redefined independently with different meanings.

## Architecture posture

Start as a clean modular system.

Do **not** introduce microservices, Kafka, Kubernetes, or distributed complexity only to make the system look "enterprise."

The design should maintain boundaries that allow components to be extracted later when real scale justifies it.

---

# 7. Repository Structure and Ownership

Target structure:

```text
twsl/
├── AGENTS.md
├── README.md
├── docs/
│   ├── PRD.md
│   ├── ARCHITECTURE.md
│   ├── API_CONTRACT.md
│   ├── DATA_MODEL.md
│   └── SECURITY.md
├── backend/
├── frontend/
└── shared/
    └── contracts/
```

## 7.1 Backend owner — Aziz

Primary ownership:

- `/backend`
- backend architecture;
- authentication server;
- device/session model;
- PostgreSQL schema and migrations;
- REST APIs;
- WebSocket/realtime gateway;
- message persistence;
- message ordering;
- synchronization;
- receipts;
- media authorization;
- push orchestration;
- backend tests;
- observability;
- rate limiting and server-side security.

## 7.2 Mobile owner — frontend partner

Primary ownership:

- `/frontend`
- React Native application;
- design system;
- navigation;
- Home;
- Chats;
- Activity;
- You;
- conversation UI;
- local SQLite persistence;
- API client integration;
- realtime client integration;
- optimistic UI;
- offline behavior;
- push handling;
- animations/interactions;
- RTL/LTR;
- accessibility;
- frontend tests.

## 7.3 Shared ownership

Shared areas:

- `/docs`
- `/shared/contracts`
- product behavior;
- OpenAPI schema;
- realtime event schema;
- shared models/enums.

Neither side may silently change a shared contract just to simplify its own implementation.

---

# 8. Consumer Navigation

The initial bottom navigation is:

1. **Home**
2. **Chats**
3. **Activity**
4. **You**

Do not add Calls, Channels, Wallet, Status, Communities, or AI as bottom-navigation items during the first product phase.

---

# 9. Home

Home is the default landing screen.

Its purpose is to help the user immediately discover and access useful businesses and services.

## 9.1 Header

The Home header contains:

- OctoChat identity/logo;
- notification entry point if required;
- primary search.

## 9.2 Search

Conceptual placeholder:

> Search businesses, services or what you need

Search must eventually support:

- business name;
- business category;
- service name;
- Arabic;
- English;
- mixed Arabic/English queries where practical.

## 9.3 Categories

Initial category set:

- Food & Dining
- Shopping
- Healthcare
- Beauty
- Travel
- Automotive
- Banking & Finance
- Telecom
- Entertainment
- More

Categories are server-configurable rather than permanently hard-coded into the mobile app.

## 9.4 Quick actions

Initial conceptual actions:

- Book
- Order
- Pay
- Get Support

The early product does **not** need full booking/payment/commerce engines merely because these actions are visible.

Until an integration exists, an action may route the user to compatible business discovery or the relevant business conversation.

Unsupported actions must never pretend a transaction was completed.

## 9.5 Your Activity

This section appears only when useful activity exists.

Examples:

- order arriving today;
- appointment tomorrow;
- booking confirmed;
- support case updated.

Every item must:

- identify the source business;
- show relevant timing/status;
- open the appropriate context or conversation.

## 9.6 Recently Used

Horizontal list of businesses the user recently:

- contacted;
- viewed;
- transacted with once future integrations exist.

## 9.7 Featured / Popular Businesses

Server-driven list of verified businesses.

This section must not turn Home into an advertising feed.

---

# 10. Business Discovery

Users can:

- search for businesses;
- browse categories;
- see verified identity;
- view business details;
- start a conversation.

## 10.1 Business result card

A business result may show:

- logo;
- official name;
- verification badge/state;
- category;
- short description;
- primary action such as `View` or `Message`.

Location-aware ranking may be introduced later and is not required for the first build.

---

# 11. Business Profile

A business is a first-class entity, not a consumer account with a fake verification badge.

A business profile may contain:

- logo;
- official business name;
- verification state;
- description;
- category;
- opening hours;
- website;
- contact details where appropriate;
- supported actions;
- primary `Message` action.

Future actions may include:

- Book;
- Order;
- Pay;
- Support;
- View catalog.

Verified and unverified identities must be visually and semantically distinguishable.

---

# 12. Chats

Chats is the user's conversation inbox.

It includes:

- person-to-person conversations;
- verified business conversations.

## 12.1 Conversation list item

A list item may display:

- avatar/logo;
- display name;
- verification badge for verified businesses;
- latest message preview;
- timestamp;
- unread count;
- mute state;
- pin state;
- draft indicator;
- typing state when active;
- delivery state for the user's latest outgoing message where useful.

## 12.2 Filters

Initial filters:

- All
- Unread
- People
- Businesses

## 12.3 New Chat

The user can:

- search registered OctoChat users;
- search businesses;
- start a new 1-to-1 conversation.

Phone-contact discovery may be added after core messaging is stable.

---

# 13. Conversation Screen

## 13.1 Required messaging features

The initial conversation experience must support:

- text messages;
- timestamps;
- clear incoming/outgoing distinction;
- optimistic sending;
- queued state;
- sending state;
- sent state;
- delivered state;
- read state;
- failed state and retry;
- reply to message;
- reactions;
- edit own message;
- delete own message;
- copy text;
- typing indicator;
- presence/online display where privacy settings permit;
- pagination/history loading;
- unread boundary;
- smooth keyboard behavior;
- reconnect behavior;
- offline queueing.

## 13.2 Media

Support:

- images;
- video attachments;
- files/documents;
- voice notes.

Large media must not travel inside WebSocket message bodies.

## 13.3 Not initially required

- voice calls;
- video calls;
- groups;
- live location;
- channels;
- stories/status;
- sticker marketplace.

---

# 14. Message Reliability Requirements

Message reliability is a product requirement.

## 14.1 Client-generated idempotency key

Each outgoing logical message must receive a globally unique `client_message_id` before first submission.

If the client retries because of timeout, reconnect, or uncertain acknowledgement, the server must recognize the same logical message and must not create a duplicate.

## 14.2 Canonical server identity

After acceptance, the backend assigns the canonical server message identity.

## 14.3 Authoritative ordering

Each conversation requires an authoritative server-controlled ordering mechanism.

Client clock timestamps alone must never define message order.

## 14.4 Outgoing state model

At minimum:

- `queued`
- `sending`
- `sent`
- `delivered`
- `read`
- `failed`

Exact wire values will be defined in shared contracts.

## 14.5 Realtime is not the source of truth

WebSockets provide live responsiveness.

They are not the only recovery mechanism.

The client must be able to recover authoritative state through synchronization/history APIs after:

- network loss;
- app suspension;
- app termination;
- missed realtime events;
- WebSocket disconnect;
- missed push;
- server restart.

## 14.6 Offline sending

When temporarily offline:

1. create the message locally;
2. persist it;
3. show queued state;
4. retry after connectivity returns;
5. reconcile using `client_message_id`;
6. never create duplicates from retry behavior.

---

# 15. Realtime Requirements

The realtime channel must eventually support at least:

- `message.created`
- `message.updated`
- `message.deleted`
- `message.delivered`
- `message.read`
- `typing.started`
- `typing.stopped`
- `presence.changed`
- `conversation.updated`

Exact event envelopes and payloads are defined outside this PRD.

Realtime events must:

- be versioned;
- have stable names;
- be documented;
- live in shared contracts;
- be safe to receive more than once where practical.

The client must tolerate reconnects and duplicate realtime events.

---

# 16. Local Mobile Persistence

The mobile app maintains a local persistent store.

It must retain enough state to:

- show the conversation list immediately;
- open previously loaded conversations without waiting for the network;
- preserve queued/unsent messages;
- preserve stable UI during reconnects;
- reconcile remote state;
- store pagination/sync metadata as required.

The mobile application should render known local state first and synchronize in the background.

---

# 17. Authentication and Device Model

## 17.1 Initial authentication

Initial consumer onboarding:

1. enter phone number;
2. receive OTP;
3. verify OTP;
4. create basic profile.

## 17.2 User profile

Initial profile:

- display name;
- profile image;
- phone number;
- username when enabled.

The phone number must not be used as the permanent internal primary identity.

## 17.3 Devices

The backend models devices separately from users from the beginning.

A single user may eventually have:

- iPhone;
- Android phone;
- tablet;
- web session;
- desktop session.

The initial release may expose limited device-management UI, but the data model must not assume one user equals one device.

---

# 18. Push Notifications

Support iOS and Android push.

Push is **not** the canonical message transport.

When appropriate, push notifies/wakes the client, and the client synchronizes authoritative state from OctoChat.

Requirements:

- push token registration;
- token refresh/revocation;
- notification preferences;
- safe behavior when logged out;
- deep linking into the relevant conversation;
- privacy controls for hiding message previews.

Sensitive/private content must not be unnecessarily exposed in push payloads.

---

# 19. Media and Voice Notes

## 19.1 Media upload flow

Expected high-level pattern:

1. client requests authorization/upload target;
2. client uploads media to the approved object-storage/media path;
3. backend records attachment metadata;
4. message references the attachment;
5. authorized recipients retrieve media.

Required behaviors:

- progress;
- retry;
- cancellation;
- type validation;
- size validation;
- authorization;
- safe file naming;
- cleanup of abandoned uploads.

## 19.2 Voice notes

Required:

- press/hold or tap-to-record interaction selected by design;
- record;
- cancel;
- send;
- playback;
- playback progress;
- duration;
- seek where practical.

Waveform visualization is desirable but secondary to reliable recording/playback.

---

# 20. Activity

Activity is a structured view of useful business events.

It is **not** a duplicate generic notification center.

Initial event concepts:

- booking confirmation/update;
- order update;
- delivery update;
- support-case update;
- generic verified-business update.

Each Activity item should be:

- attributable to a business;
- time-aware;
- actionable;
- linked to relevant context.

During early internal development, demo/seed activity data is acceptable as long as it is clearly development data and not presented as a completed integration.

---

# 21. You / Settings

Initial settings areas:

- profile;
- profile image;
- display name;
- username when available;
- QR/profile sharing when available;
- notifications;
- privacy;
- blocked users;
- devices/sessions;
- language;
- appearance;
- help;
- privacy/legal information;
- logout.

Changing between Arabic and English must correctly update directionality and layout.

---

# 22. Business Messaging Foundation

Business messaging must use the same core messaging infrastructure as consumer messaging.

Do not create a completely separate fake business chat engine.

## 22.1 Business entity

High-level business properties:

- business ID;
- official name;
- logo;
- verification state;
- description;
- category;
- status;
- optional business locations;
- supported capabilities.

## 22.2 Verified identity

Verification must be backed by server-side state and authorization.

The client must never be able to self-assign verification.

## 22.3 Business conversation

A consumer can start a conversation with a business.

The conversation uses the standard messaging model while supporting future business-specific capabilities.

---

# 23. Future Business Console

A web-based business console is part of the OctoChat product direction but is not required to block the first consumer messaging build.

Planned capabilities:

- shared inbox;
- multiple employees;
- roles and permissions;
- conversation assignment;
- open/pending/closed states;
- internal notes;
- labels;
- customer context;
- AI handling;
- human handoff;
- analytics;
- saved replies;
- business hours;
- automation;
- API credentials;
- webhook management.

This console will be specified separately before implementation.

---

# 24. AI Direction

AI is a planned native business capability, not a requirement for the first reliable-message milestone.

Future AI capabilities may include:

- business knowledge agents;
- automated customer support;
- intent classification;
- human handoff;
- conversation summarization;
- translation;
- suggested replies;
- voice-note transcription;
- natural-language chat search.

AI must not be allowed to bypass authorization, invent transactional outcomes, or silently perform sensitive actions.

---

# 25. Rich Business Messages

Future business conversation components may include:

- quick replies;
- CTA buttons;
- menus/lists;
- cards;
- product cards;
- forms;
- date/time selection;
- booking flows;
- payment requests;
- receipts;
- order cards;
- tickets;
- authentication prompts.

These should eventually be represented by versioned structured message types rather than arbitrary frontend-only layouts.

---

# 26. Future OctoChat Business API

The business API is a core strategic direction.

Future capabilities include:

- send message;
- receive inbound-message webhooks;
- delivery/read webhooks;
- rich message types;
- media;
- business identity;
- customer/conversation lookup subject to permissions;
- AI/human routing;
- booking/commerce/payment integrations.

The external API must use the same canonical messaging semantics as first-party clients.

---

# 27. Privacy and Security Requirements

Security architecture will receive its own specification.

The product requirements are:

- TLS for all network transport;
- secure authentication and session handling;
- secure server-side secret management;
- encryption at rest for sensitive backend storage where appropriate;
- strict authorization on every protected resource;
- rate limiting;
- abuse controls;
- account/session revocation;
- safe logs and telemetry;
- no production secrets committed to Git;
- least-privilege service access;
- user blocking/reporting;
- data deletion/account deletion flows before public production launch;
- privacy-respecting analytics.

## 27.1 E2EE

Private messaging is intended to support end-to-end encryption.

The E2EE architecture must be separately designed before claiming production E2EE.

Requirements:

- established protocol/library;
- device-aware key model;
- no homemade cryptography;
- defined key rotation/recovery behavior;
- multi-device compatibility plan;
- encrypted attachment strategy;
- external security review before claiming superiority over mature encrypted messengers.

OctoChat must not market itself as "more secure than WhatsApp" merely because encryption exists.

## 27.2 Logging

Production observability must avoid logging private message bodies, authentication secrets, OTPs, cryptographic keys, or sensitive personal content.

---

# 28. Abuse, Spam, and User Safety

Messaging infrastructure must assume abuse will occur.

Required direction:

- block user;
- report user/business;
- rate limiting;
- account creation abuse controls;
- OTP abuse controls;
- business initiation rules;
- spam controls;
- moderation/escalation tooling appropriate to the product;
- auditability of sensitive business actions.

Businesses must not receive unlimited ability to blast users merely because they exist on OctoChat.

Consent, relevance, unsubscribe/mute controls, and applicable legal requirements must be respected.

---

# 29. Regulatory / Commercial Launch Gate

Engineering may proceed before commercial launch clearance.

Public/commercial launch in Kuwait must not occur until Octopus has completed appropriate legal/regulatory review, including the applicable CITRA telecommunications/privacy requirements.

Expansion into another GCC jurisdiction requires jurisdiction-specific review.

Calling/VoIP features are specifically excluded from the first launch commitment and must undergo separate regulatory review before release.

---

# 30. Accessibility

The mobile application should support:

- scalable text;
- screen-reader labels;
- adequate touch targets;
- non-color-only status communication;
- logical focus order;
- accessible message actions;
- RTL accessibility behavior.

Accessibility regressions should be treated as product bugs.

---

# 31. Performance and Quality Targets

These are engineering targets, not marketing guarantees.

## 31.1 Perceived performance

- cached chat list should appear immediately without waiting for a network round trip;
- sending a text should create the local outgoing bubble effectively instantly;
- navigation and keyboard interactions should remain smooth;
- reconnecting should not freeze the UI.

## 31.2 Reliability qualification

Before the messaging core is considered ready, test at minimum:

- two real devices;
- app kill/relaunch;
- Wi‑Fi on/off;
- Wi‑Fi to cellular transition;
- recipient offline and later online;
- sender offline and later online;
- request timeout followed by retry;
- duplicate submission of the same `client_message_id`;
- 100 rapid sequential messages;
- history pagination;
- read receipts;
- queued outgoing messages;
- push notification open/deep link;
- Arabic-only messages;
- English-only messages;
- mixed Arabic/English messages.

Pass condition:

- zero lost acknowledged messages;
- zero duplicate logical messages from retry;
- deterministic authoritative ordering;
- correct unread counts after resync;
- correct delivery/read reconciliation.

---

# 32. Observability

Backend and client behavior must be diagnosable without inspecting private message content.

Future production observability should include:

- structured logs;
- request IDs;
- trace/correlation IDs;
- metrics;
- error reporting;
- WebSocket connection metrics;
- send/ack latency;
- synchronization failures;
- push failures;
- media upload failures;
- database health;
- queue/job health;
- abuse/rate-limit events.

---

# 33. Product Analytics

Privacy-respecting analytics may measure:

- onboarding completion;
- Home search use;
- business profile views;
- new conversations;
- message send success/failure;
- message latency aggregate;
- retention;
- Activity interaction;
- business discovery conversion.

Do not use private message body content as a general analytics surface.

---

# 34. High-Level Core Entities

The detailed schema will live in `DATA_MODEL.md`.

Expected high-level entities include:

- User
- UserProfile
- Device
- Session
- PushToken
- Business
- BusinessLocation
- Conversation
- ConversationParticipant
- Message
- MessageReceipt
- MessageReaction
- Attachment
- ActivityItem
- Block
- Report

Future business-platform entities may include:

- BusinessAgent
- BusinessRole
- Assignment
- InternalNote
- Label
- AIConfiguration
- APIClient
- WebhookEndpoint

Names may be refined in the data-model specification, but frontend and backend must then use the agreed canonical names.

---

# 35. First Executable Delivery Sequence

This is the order in which the product should become real.

## Stage 0 — Repository and contracts

- canonical PRD;
- repository structure;
- `AGENTS.md`;
- architecture document;
- data model;
- OpenAPI contract;
- realtime-event contract;
- local development setup;
- CI;
- formatting/linting/testing baseline.

## Stage 1 — Identity

- phone onboarding;
- OTP development flow/provider abstraction;
- user profile;
- device registration;
- authenticated session;
- logout/revocation.

## Stage 2 — 1:1 messaging core

- create/find user;
- create 1:1 conversation;
- list conversations;
- WebSocket authentication;
- send text;
- persist text;
- receive text realtime;
- acknowledgement;
- idempotency;
- authoritative ordering.

## Stage 3 — synchronization and receipts

- history API;
- cursor/sequence synchronization;
- offline recovery;
- queued sends;
- delivered state;
- read state;
- unread counts;
- reconnect behavior.

## Stage 4 — premium chat UX

- reply;
- reactions;
- edit/delete;
- typing;
- presence;
- pagination;
- local SQLite rendering;
- polished keyboard behavior;
- Arabic/RTL qualification.

## Stage 5 — push and media

- push registration;
- background notification behavior;
- image upload;
- file upload;
- video attachment;
- voice notes.

## Stage 6 — business discovery

- business entity;
- verification state;
- Home;
- search;
- categories;
- business profile;
- start business conversation;
- recent businesses.

## Stage 7 — Activity

- activity model;
- Activity tab;
- structured business updates;
- linking activity to conversations/context.

## Stage 8 — security hardening / E2EE track

E2EE architecture must be designed deliberately and integrated without violating reliability/sync requirements.

Security work is continuous; this stage refers to the dedicated encrypted-messaging track, not the beginning of security.

## Stage 9 — business platform

After the consumer messaging foundation is reliable:

- business web console;
- multi-agent inbox;
- human handoff;
- AI;
- business APIs/webhooks;
- rich messages;
- first Octopus integration.

---

# 36. Explicitly Deferred Features

Do not allow these features to derail the messaging foundation:

- group chat;
- channels;
- communities;
- stories/status;
- voice calls;
- video calls;
- screen sharing;
- P2P wallet;
- P2P transfers;
- social feed;
- public creator platform;
- ads;
- full commerce marketplace;
- full visual workflow builder;
- native desktop applications.

Deferred does not mean rejected.

It means they require their own specifications after the core is trustworthy.

---

# 37. Definition of Done for the Messaging Foundation

The messaging foundation is **not done** merely because two simulators can exchange text.

It is done when:

1. two real physical devices can authenticate;
2. either user can start a 1:1 conversation;
3. both can exchange messages in realtime;
4. messages are persisted;
5. retries do not duplicate logical messages;
6. ordering remains correct;
7. recipient can be offline and receive/sync later;
8. sender can send while offline and recover;
9. killing/reopening either app does not lose acknowledged state;
10. delivered/read states reconcile correctly;
11. unread counts recover correctly;
12. chat history renders from local persistence;
13. push works when the app is backgrounded/closed as supported by the OS;
14. Arabic, English, and mixed messages render correctly;
15. error states are understandable and retryable;
16. automated tests cover critical server and client state transitions;
17. basic observability can diagnose failures without reading private message content.

---

# 38. Collaboration Rules

This repository is shared by two engineers and multiple Codex sessions.

## 38.1 Main branch

`main` represents integrated working code.

Do not use `main` as a personal scratch branch.

## 38.2 Branch naming

Examples:

Backend:

- `backend/auth`
- `backend/messaging-core`
- `backend/sync`
- `backend/media`

Mobile:

- `mobile/app-shell`
- `mobile/home`
- `mobile/chat-list`
- `mobile/conversation`

Shared:

- `contracts/messaging-v1`
- `docs/architecture-v1`

## 38.3 Pull requests

Work should enter `main` through pull requests.

A PR should:

- have one clear purpose;
- include tests where relevant;
- describe contract changes;
- avoid unrelated refactors;
- pass CI.

## 38.4 Ownership

Backend work should not casually modify mobile implementation.

Mobile work should not casually modify backend implementation.

Shared-contract changes require explicit review because they may affect both owners.

## 38.5 Contract-first parallel work

The frontend developer must not wait for the backend to be fully implemented.

Workflow:

1. agree on contract;
2. commit contract;
3. mobile builds against generated types/mocks;
4. backend implements the same contract;
5. integration replaces mocks without redesigning the feature.

## 38.6 No unilateral API invention

If mobile needs a field/endpoint/event that does not exist:

- propose/update the shared contract first;
- document why;
- review the impact;
- then implement.

If backend wants to rename/remove/change a field:

- update the contract deliberately;
- account for compatibility;
- do not silently break mobile.

---

# 39. Source-of-Truth Hierarchy

When documents overlap, use this hierarchy:

1. **PRD** — product intent and behavior.
2. **Architecture / Security docs** — system design constraints.
3. **OpenAPI + realtime schemas** — exact machine interface.
4. **Data model docs/migrations** — canonical persisted model.
5. **Code** — implementation of the above.

A disagreement between layers is a bug to resolve, not an excuse to choose whichever version is convenient.

---

# 40. First Product Acceptance Scenario

A canonical end-to-end scenario for the team:

1. Aziz installs OctoChat on Device A.
2. Partner installs OctoChat on Device B.
3. Both complete authentication/profile setup.
4. Aziz searches for Partner.
5. Aziz opens a 1:1 conversation.
6. Aziz sends `Hello`.
7. Local bubble appears immediately.
8. Server accepts the message exactly once.
9. Partner receives it in realtime.
10. Aziz sees delivery state update.
11. Partner opens the conversation.
12. Aziz sees read state update.
13. Device B goes offline.
14. Aziz sends additional messages.
15. Device B reconnects.
16. Missing messages synchronize in correct order without duplicates.
17. Device A is killed and relaunched.
18. Existing conversation renders immediately from local storage and then reconciles.
19. Both send Arabic/English mixed messages successfully.
20. A verified demo business appears on Home.
21. Aziz opens the business profile and starts a conversation using the same underlying messaging system.

That scenario is the first meaningful proof that OctoChat is becoming a real platform.

---

# 41. Product Roadmap Direction After the Foundation

Once the core is trustworthy, OctoChat can expand into:

- business shared inbox;
- AI agents and human handoff;
- rich interactive messages;
- OctoBooking;
- OctoPay;
- OctoCommerce;
- business APIs and webhooks;
- groups;
- advanced privacy;
- multi-device;
- calls where legally approved;
- channels/communities where product evidence supports them;
- broader GCC rollout.

These expansions must reuse the messaging foundation rather than creating disconnected mini-products.

---

# 42. Final Rule

The objective is not to produce the largest feature checklist.

The objective is to build a communication product that users and businesses can trust.

When forced to choose between:

- more features; or
- correct delivery, fast UX, security, clean contracts, maintainability, and trust;

choose the second.

Every time.
