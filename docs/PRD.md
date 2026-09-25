# **Bozcord: Product Requirements Document (PRD)**

# **1\. Executive Summary & Product Vision**

**Product Name:** Bozcord  
**Platform:** Desktop (Windows, macOS, Linux)  
**Monetization:** 100% Free and Open Source Software (FOSS), Ad-Free.

Bozcord is envisioned as an ultra-performant, minimalist, and fully open-source alternative to Discord. Triggered by restrictive access to existing communication tools, Bozcord aims to provide a resilient, user-owned platform primarily for gamers and community builders. Written entirely in Rust, Bozcord leverages the language's memory safety, concurrency, and performance characteristics to deliver an exceptionally lightweight client and a high-throughput backend.

The core philosophy revolves around three pillars:

- **Extreme Performance:** Minimal resource footprint (CPU/RAM) enabling seamless operation alongside resource-intensive applications (e.g., modern gaming).
- **Absolute Minimalism:** An intuitive, decluttered, and highly customizable UI/UX.
- **FOSS & User Autonomy:** A completely free platform with a roadmap leading to self-hostable infrastructure, ensuring resilience against external restrictions.

# **2\. Target Personas**

**Persona 1: The Resource-Conscious Gamer**

- **Profile:** Spends significant time in resource-heavy games.
- **Needs:** A voice and text client that uses near-zero CPU and RAM, ensuring zero impact on in-game framerates.
- **Frustrations:** Bloated Electron-based clients that consume excessive background resources and force unwanted features.

**Persona 2: The Community Architect**

- **Profile:** Runs small to medium-sized interest groups or gaming guilds.
- **Needs:** Reliable text channels, voice rooms, and straightforward moderation tools without paywalls.
- **Frustrations:** Feature locking behind premium tiers, platform instability, and lack of data ownership.

**Persona 3: The Privacy-Centric Tech Enthusiast**

- **Profile:** Values open-source software and data sovereignty.
- **Needs:** Transparent source code, secure communication protocols, and the ability to self-host servers in the future.

# **3\. Technical Architecture & Stack Breakdown**

Bozcord is designed as a distributed system, initially operating in a centralized topology for the MVP, with strict architectural boundaries to allow seamless transition to a federated/self-hosted model in Phase 2\.

Core Language: Rust 100% (Client and Server)

## **3.1. Client-Side Stack**

- **Frontend GUI (Slint):** Selected for its exceptional performance and low overhead compared to Electron or Tauri (when heavily reliant on web views). Slint compiles natively, providing a fluid, responsive UI with minimal memory consumption, directly aligning with the USP.
- **Asynchronous Runtime (Tokio):** The industry standard for Rust async. Essential for managing concurrent WebSocket connections, voice data streams, and UI updates without blocking the main thread.

## **3.2. Server-Side Stack**

- **Backend Framework (Axum):** Built on top of `tokio` and `hyper`, Axum provides a robust, highly ergonomic routing and middleware system. Its strong typing and macro-free design make it ideal for building a stable REST API and managing WebSocket upgrades for real-time communication.
- **Database (PostgreSQL via sqlx):** PostgreSQL ensures ACID compliance and robust relational data management for users, servers, and channels. `sqlx` provides compile-time checked SQL queries, significantly reducing runtime errors and improving security against SQL injection.
- **In-Memory Datastore / PubSub (Redis):** Crucial for managing presence (online/offline status), routing real-time chat messages via PubSub across potentially horizontally scaled Axum instances, and caching frequently accessed data (e.g., user profiles).

## **3.3. Architectural Considerations for Self-Hosting**

To ensure future self-hosting capabilities, the architecture must decouple state from the application servers. All transient states (presence, active voice sessions) will reside in Redis, and persistent state in PostgreSQL. The server binary will be packaged via Docker, allowing a user to spin up the Axum server, Postgres, and Redis using a single `docker-compose.yml` file.

# **4\. Phased Feature Rollout**

## **Phase 1: Minimum Viable Product (MVP)**

_Goal: Establish core communication primitives with exceptional stability._

- **Authentication:** Basic user registration and login (JWT based).
- **Servers (Guilds):** Creation and joining of servers via invite links.
- **Text Channels:** Real-time text messaging using WebSockets. Basic Markdown support.
- **Voice Rooms:** Low-latency voice communication. (Requires research into WebRTC vs. custom UDP protocols in Rust).
- **Direct Messaging (DMs):** 1-on-1 text communication.
- **Presence:** Online/Offline status indicators.

## **Phase 2: Media & Autonomy**

_Goal: Feature parity with standard communication apps and infrastructure independence._

- **Media Sharing:** File uploads (images, documents) with size constraints.
- **Screen Sharing:** Low-latency desktop capture and streaming.
- **Self-Hosting Package:** Release of the official Dockerized server environment with configuration documentation.
- **Customization:** Advanced UI theming (Dark/Light/Custom palettes) within Slint.

# **5\. UI/UX Guidelines**

- **Visual Language:** Flat, modern, with high contrast for readability.
- **Information Architecture:** Left-aligned server bar, secondary channel sidebar, central chat view, optional right-aligned member list.
- **Interaction Design:** Keyboard-centric navigation capabilities. Instant visual feedback for actions (message sent, voice joined) masking any network latency.
- **Anti-Patterns to Avoid:** No intrusive tooltips, no gamification of usage, no pop-up advertisements or premium tier upsells.

# **6\. Security & Privacy R\&D Plan**

As a platform positioning itself against commercial alternatives, security is a primary deliverable. The following protocols require dedicated R\&D during the MVP phase:

1. **Transport Security:** Enforce TLS 1.3 for all REST API and WebSocket connections.
2. **Voice Protocol Security:** Research and implement DTLS (Datagram Transport Layer Security) or SRTP (Secure Real-time Transport Protocol) for encrypting voice traffic over UDP. The `webrtc-rs` crate is a strong candidate for investigation.
3. **End-to-End Encryption (E2EE) for DMs:** Investigate the Signal Protocol (via crates like `libsignal-protocol-rust` if licensing permits, or alternative implementations) to ensure DMs are cryptographically secure and unreadable by the server.
4. **Data Minimization:** Design the PostgreSQL schema to store only essential data. Implement routine purging of ephemeral data (e.g., old presence logs).
5. **Rate Limiting & Abuse Prevention:** Implement strict IP and user-based rate limiting via Redis to mitigate DDoS attempts and spam.

# **7\. Realistic Development Timeline & Milestones**

_Context: Part-time solo developer (4th-year medical student). Timeline optimized for consistency over speed, targeting a 6-8 month MVP._

- **Month 1: Foundation & R\&D:** Set up repository, CI/CD (GitHub Actions). Finalize database schema (PostgreSQL) and initialize Axum backend. R\&D: Voice streaming architecture in Rust.
- **Month 2: Backend Core:** Implement Authentication API (JWT). Implement Server/Channel management APIs. Integrate Redis for PubSub and session management.
- **Month 3: Frontend Scaffolding & Comms:** Initialize Slint desktop project. Establish WebSocket connection for real-time text delivery. Build basic UI layout (Servers, Channels, Chat Area).
- **Month 4: Voice Implementation (The Hard Part):** Implement voice signaling server. Integrate audio capture/playback in the client. Achieve basic 1-to-1 or small group voice communication.
- **Month 5: Integration & Polish:** Connect Slint UI to backend APIs (Auth, Server creation, Messaging). Implement Direct Messaging logic. Refine UI responsiveness and error handling.
- **Months 6-8: Testing, Security & MVP Launch:** Implement Security R\&D findings (Rate limiting, secure voice transport). Extensive profiling and memory leak testing (Valgrind/Rust tooling). Closed Alpha testing with a small group of users. Open-source release (GitHub) with comprehensive documentation.

# **8\. Success Metrics (MVP)**

- **Performance:** Client consumes \< 50MB RAM at idle and \< 100MB during active voice chat. Client CPU usage \< 2% background, \< 5% active.
- **Latency:** Message delivery \< 100ms globally (excluding extreme network conditions). Voice latency \< 50ms locally.
- **Stability:** Zero crashes during a 48-hour continuous voice session test.
- **Adoption:** Successful deployment of the open-source repository with clear setup instructions, resulting in \> 50 initial GitHub stars.
