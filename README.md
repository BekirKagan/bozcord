<div align="center">
  <img src="https://via.placeholder.com/800x200?text=Bozcord+Banner+Placeholder" alt="Bozcord Banner">
</div>

# <img src="https://via.placeholder.com/50?text=Logo" alt="Bozcord Logo" width="30" height="30"> Bozcord ⚡

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![Repository](https://img.shields.io/badge/GitHub-Repository_Placeholder-black.svg)](https://github.com/BekirKagan/bozcord)

> An ultra-performant, minimalist, and fully open-source alternative to Discord, written entirely in Rust.[cite: 1]

Bozcord is a resilient, user-owned communication platform designed primarily for gamers, community builders, and privacy-centric individuals[cite: 1]. Born out of a need for reliable communication in regions with restrictive access to existing tools, Bozcord delivers an exceptionally lightweight client and a high-throughput backend without the bloat of modern Electron apps[cite: 1].

## ✨ Why Bozcord?

- **Extreme Performance:** Engineered to use near-zero CPU and RAM[cite: 1]. With a footprint of < 50MB RAM at idle, it runs seamlessly alongside resource-heavy games without impacting framerates[cite: 1].
- **Absolute Minimalism:** An intuitive, decluttered UI with no intrusive tooltips, pop-up advertisements, or premium upsells[cite: 1].
- **100% FOSS & User Autonomy:** Completely free and open-source[cite: 1]. The architecture is designed to transition from a centralized MVP to a fully self-hostable infrastructure[cite: 1].
- **Privacy First:** Built with a focus on data sovereignty, secure transport (TLS 1.3), and upcoming End-to-End Encryption (E2EE) for direct messages[cite: 1].

## 🛠️ Technology Stack

Bozcord is proudly built with 100% Rust across both the client and server[cite: 1].

**Client-Side:**

- **GUI:** [Slint](https://slint.dev/) - A natively compiled toolkit providing a fluid, responsive UI with minimal memory consumption[cite: 1].
- **Async Runtime:** [Tokio](https://tokio.rs/) - For managing concurrent WebSockets and voice streams without blocking the main thread[cite: 1].

**Server-Side:**

- **Backend Framework:** [Axum](https://github.com/tokio-rs/axum) - Robust, macro-free routing for the REST API and WebSocket upgrades[cite: 1].
- **Database:** PostgreSQL with `sqlx` for compile-time checked, ACID-compliant relational data management[cite: 1].
- **In-Memory Datastore:** Redis for managing online presence, real-time PubSub routing, and rate-limiting[cite: 1].

## 🚀 Roadmap

### Phase 1: Minimum Viable Product (MVP) [Current Focus]

- [ ] **Authentication:** JWT-based user registration and login[cite: 1].
- [ ] **Servers (Guilds):** Creation and joining via invite links[cite: 1].
- [ ] **Text Channels:** Real-time WebSockets messaging with basic Markdown[cite: 1].
- [ ] **Voice Rooms:** Low-latency 1-to-1 and group voice communication[cite: 1].
- [ ] **Direct Messaging (DMs):** 1-on-1 text communication[cite: 1].

### Phase 2: Media & Autonomy

- [ ] **Media Sharing:** File and image uploads[cite: 1].
- [ ] **Screen Sharing:** Low-latency desktop capture[cite: 1].
- [ ] **Self-Hosting:** Official Dockerized server environment release (`docker-compose.yml`)[cite: 1].
- [ ] **Customization:** Advanced Slint UI theming (Dark/Light/Custom)[cite: 1].

## 💻 Getting Started (Development)

_(Instructions for cloning, setting up PostgreSQL/Redis via Docker, and running `cargo build` will be added here once the MVP architecture is finalized.)_

## 🤝 Contributing

Bozcord is currently in its **Phase 1 (MVP) development cycle**. As a solo project managed by a medical student in limited free time[cite: 1], **Pull Requests are not being accepted at this stage**. This ensures strict architectural control and a streamlined workflow while the foundational Rust codebase is established.

Once Phase 1 is complete and the project transitions to Phase 2 (Media & Autonomy)[cite: 1], contributing guidelines will be published and community PRs will be warmly welcomed.

## 🛡️ License

This project is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**. This ensures that any modifications to the backend, even if run as a service over a network, must be open-sourced. See the `LICENSE` file for details.
