<h1 align="center">Gustavo Hitomi</h1>

<p align="center">
  <em>cs @ usp · I like data engineering and rust, trying to build a startup + HFT</em>
</p>

---

### 👋 About

Most of what I build falls into one of two buckets:

- **Systems that can't be wrong** — event-sourced medical software, deterministic triage engines, trading bots that sign real transactions. When a mistake costs money or matters to a patient.
- **Tools for games I actually play** — Magic: The Gathering trackers, Dota 2 analyzers. Small scope, real users, shipped.

Mostly **Rust**, **Python**, and **TypeScript**.

---

### 🛠 Tech

**Languages**

![Rust](https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=rust&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)

**Backend & data**

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=for-the-badge&logo=supabase&logoColor=white)
![Tokio](https://img.shields.io/badge/Tokio-000000?style=for-the-badge&logo=rust&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)

**Frontend**

![React](https://img.shields.io/badge/React_19-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![PWA](https://img.shields.io/badge/PWA-5A0FC8?style=for-the-badge&logo=pwa&logoColor=white)
![WebAssembly](https://img.shields.io/badge/WebAssembly-654FF0?style=for-the-badge&logo=webassembly&logoColor=white)

**Infra & tooling**

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Neovim](https://img.shields.io/badge/Neovim-57A143?style=for-the-badge&logo=neovim&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)

---

### 📦 Featured work

#### 🏥 Zela — post-operative patient follow-up `private`

Follow-up platform for small surgical teams, built as a pilot for a LASIK, orthopedists and general surgery. Patients answer short structured check-ins on the days their protocol schedules; a deterministic rules engine scores each one 🟢/🟡/🔴, and anything not green lands in a risk-ordered queue where the surgeon reviews it and records whether they agree with the triage.

The design decisions I'm proudest of:

- **Event-sourced for real.** Every state change is an immutable row in an append-only `events` table. `UPDATE` and `DELETE` are *revoked* on the application's database role and blocked by a trigger, everything queryable is a projection that `rebuild_all()` can reconstruct from scratch.
- **Protocols are data, not code.** A protocol is a versioned JSON record carrying its check-in questions, schedule, photo requirement, and triage ruleset. Supporting a new specialty means seeding a row. The patient form, the bot's questions, and the surgeon's queue all render from it.
- **Triage is deterministic.** Rules are `{field, op, value}` conditions; the score is the most severe rule that fired. VLM AI in v0.3, when one photo arrives, it may only *escalate* a score, never lower it.
- **The channel is an abstraction.** The domain says "request check-in"; an adapter decides whether that's Telegram (for prototyping, WhatsApp API burocracy sucks), the web app, or WhatsApp later. The core never names a provider.

`FastAPI` · `PostgreSQL` · `React 19` · `TypeScript` · `Docker Compose` · `pytest` · `axe-core`

#### 📈 polymarket-copy-bot-rs — copy-trading bot `private`

High-performance Polymarket copy-trading bot in Rust. Polls the Data API for a target wallet's fills, mirrors them through the CLOB with EIP-712-signed orders, and tracks positions in migration-managed SQLite. Handles the unglamorous parts that decide whether a bot survives contact with production: on-chain USDC approval at startup, floor-snapping when the wallet is empty, and downgrading expired-market 404s so the logs stay readable. Made a lot of money from this, but it's hard to find good players to copy >.<

`Rust` · `tokio` · `polymarket-client-sdk` · `alloy` · `sqlx` · `reqwest`

#### 🃏 [fdc-tracker](https://github.com/hitomi1/fdc-tracker) — offline MTG draft tracker `public`

Event tracker for the [*Fora da Caixa* MTG Arena community](https://www.instagram.com/foradacaixamtg). Log for Limited results, no server or account required by default; everything lives in `localStorage` and the app is a fully installable offline PWA. Optional Supabase login syncs events across devices. Includes a performance tab with rolling win-rate charts, full reward tables with net-gem math per record, 17Lands import, and a guided first-run tour.

`React 19` · `TypeScript` · `Vite` · `Supabase` · `PWA` · `gh-pages`

#### 🎮 [dotabuff-v2](https://github.com/hitomi1/dotabuff-v2) — live Dota 2 match analyzer `public`

Local web app that identifies all 10 players the moment your match begins: rank, ranked win rate, top heroes, and recent matches. With zero manual input and no browser extension. Dota 2 pushes live state to a local server via **Game State Integration**; player IDs are resolved through a three-tier fallback (Steam `GetRealtimeStats` → STRATZ GraphQL → OpenDota polling), stats are fetched concurrently from OpenDota, and results stream to the browser over Server-Sent Events. You can easily identify the bad players on your team and on the enemy team... Good ones are inexistent.

`Python` · `Flask` · `SSE` · `GSI` · `OpenDota` · `STRATZ`

#### ⚙️ [dotfiles](https://github.com/hitomi1/dotfiles) — my environment `public`

Neovim (Lua), kitty, zsh + powerlevel10k, and a one-shot `install.sh`. Reproducible from a bare machine. Needs update since Claude broke my entire setup.

---

### 🌱 Open source

- **[phase-rs/phase](https://github.com/phase-rs/phase)** — contributed a rules-parser fix to this Rust/WASM Magic: The Gathering engine ([#6540](https://github.com/phase-rs/phase/pull/6540)): scoped `"that player"` to the *triggering* player for Aura and Equipment damage triggers.

---

### 📊 Stats

<p align="center">
    [![GitHub Streak](https://streak-stats.demolab.com?user=hitomi1&theme=dark)](https://git.io/streak-stats)
</p>

<p align="center">
  <img alt="Streak" src="https://streak-stats.demolab.com/?user=hitomi1&theme=tokyonight&hide_border=true&background=00000000&ring=7aa2f7&fire=bb9af7&currStreakLabel=7aa2f7">
</p>

<p align="center"><sub>Language stats reflect public repositories — a good chunk of my Rust and Python lives in private ones.</sub></p>

---

### 📫 Reach me

<p align="center">
  <a href="mailto:gustavohitomi@gmail.com"><img alt="Email" src="https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white"></a>
  <a href="https://github.com/hitomi1"><img alt="GitHub" src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"></a>
</p>
