<h1 align="center">Gustavo Hitomi</h1>

<p align="center">
  <em>CS undergrad at USP · I build systems where correctness is the feature</em>
</p>

<p align="center">
  <img alt="Location" src="https://img.shields.io/badge/São_Carlos,_SP-Brazil-009C3B?style=flat-square&labelColor=1a1b27">
  <img alt="University" src="https://img.shields.io/badge/ICMC-USP-1094AB?style=flat-square&labelColor=1a1b27">
  <a href="https://github.com/hitomi1?tab=followers"><img alt="Followers" src="https://img.shields.io/github/followers/hitomi1?style=flat-square&labelColor=1a1b27&color=7aa2f7"></a>
  <img alt="Profile views" src="https://komarev.com/ghpvc/?username=hitomi1&style=flat-square&color=7aa2f7&label=profile+views">
</p>

---

### 👋 About

I'm a computer science student at the **Instituto de Ciências Matemáticas e de Computação (ICMC–USP)** in São Carlos, Brazil. Most of what I build falls into one of two buckets:

- **Systems that can't be wrong** — event-sourced medical software, deterministic triage engines, trading bots that sign real transactions. When a mistake costs money or matters to a patient, I reach for append-only data, immutable state, and rules that are data rather than code.
- **Tools for games I actually play** — Magic: The Gathering trackers, Dota 2 analyzers. Small scope, real users, shipped.

Mostly **Rust**, **Python**, and **TypeScript**. I like PWAs that work with the plane in airplane mode, Postgres schemas that refuse invalid writes at the database level, and CI gates that fail loudly.

---

### 🛠 Tech

**Languages**

![Rust](https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=rust&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Lua](https://img.shields.io/badge/Lua-2C2D72?style=for-the-badge&logo=lua&logoColor=white)
![C](https://img.shields.io/badge/C-A8B9CC?style=for-the-badge&logo=c&logoColor=black)

**Backend & data**

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
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

Follow-up platform for small surgical teams, built as a pilot for a LASIK + general surgery practice. Patients answer short structured check-ins on the days their protocol schedules; a deterministic rules engine scores each one 🟢/🟡/🔴, and anything not green lands in a risk-ordered queue where the surgeon reviews it and records whether they agree with the triage.

The design decisions I'm proudest of:

- **Event-sourced for real.** Every state change is an immutable row in an append-only `events` table. `UPDATE` and `DELETE` are *revoked* on the application's database role and blocked by a trigger — everything queryable is a projection that `rebuild_all()` can reconstruct from scratch.
- **Protocols are data, not code.** A protocol is a versioned JSON record carrying its check-in questions, schedule, photo requirement, and triage ruleset. Supporting a new specialty means seeding a row. The patient form, the bot's questions, and the surgeon's queue all render from it.
- **Triage is deterministic.** Rules are `{field, op, value}` conditions; the score is the most severe rule that fired. No image AI in v0 — and when one arrives, it may only *escalate* a score, never lower it.
- **The channel is an abstraction.** The domain says "request check-in"; an adapter decides whether that's Telegram, the web app, or WhatsApp later. The core never names a provider.

`FastAPI` · `PostgreSQL` · `React 19` · `TypeScript` · `Docker Compose` · `pytest` · `axe-core`

#### 📈 polymarket-copy-bot-rs — copy-trading bot `private`

High-performance Polymarket copy-trading bot in Rust. Polls the Data API for a target wallet's fills, mirrors them through the CLOB with EIP-712-signed orders, and tracks positions in migration-managed SQLite. Handles the unglamorous parts that decide whether a bot survives contact with production: on-chain USDC approval at startup, floor-snapping when the wallet is empty, and downgrading expired-market 404s so the logs stay readable.

`Rust` · `tokio` · `polymarket-client-sdk` · `alloy` · `sqlx` · `reqwest`

#### 🃏 [fdc-tracker](https://github.com/hitomi1/fdc-tracker) — offline MTG draft tracker `public`

Event tracker for the *Fora da Caixa* MTG Arena community. Log Premier, Traditional, Quick, and Sealed results — no server or account required by default; everything lives in `localStorage` and the app is a fully installable offline PWA. Optional Supabase login syncs events across devices. Includes a performance tab with rolling win-rate charts, full reward tables with net-gem math per record, 17Lands import, and a guided first-run tour.

`React 19` · `TypeScript` · `Vite` · `Supabase` · `PWA` · `gh-pages`

#### 🎮 [dotabuff-v2](https://github.com/hitomi1/dotabuff-v2) — live Dota 2 match analyzer `public`

Local web app that identifies all 10 players the moment your match begins — rank, ranked win rate, top heroes, and recent matches — with zero manual input and no browser extension. Dota 2 pushes live state to a local server via **Game State Integration**; player IDs are resolved through a three-tier fallback (Steam `GetRealtimeStats` → STRATZ GraphQL → OpenDota polling), stats are fetched concurrently from OpenDota, and results stream to the browser over Server-Sent Events.

`Python` · `Flask` · `SSE` · `GSI` · `OpenDota` · `STRATZ`

#### ⚙️ [dotfiles](https://github.com/hitomi1/dotfiles) — my environment `public`

Neovim (Lua), kitty, zsh + powerlevel10k, and a one-shot `install.sh`. Reproducible from a bare machine.

---

### 🌱 Open source

- **[phase-rs/phase](https://github.com/phase-rs/phase)** — contributed a rules-parser fix to this Rust/WASM Magic: The Gathering engine ([#6540](https://github.com/phase-rs/phase/pull/6540)): scoped `"that player"` to the *triggering* player for Aura and Equipment damage triggers.
- **[fossguild/sucury](https://github.com/fossguild/sucury)** — fixed a movement bug letting the snake reverse into itself ([#113](https://github.com/fossguild/sucury/pull/113)).
- **[gelos-icmc/site](https://github.com/gelos-icmc/site)** — contributor to GELOS, the free software group at ICMC–USP.

---

### 📊 Stats

<p align="center">
  <img height="165" alt="Gustavo's GitHub stats" src="https://github-readme-stats.vercel.app/api?username=hitomi1&show_icons=true&include_all_commits=true&theme=tokyonight&hide_border=true&bg_color=00000000&title_color=7aa2f7&icon_color=bb9af7">
  <img height="165" alt="Top languages" src="https://github-readme-stats.vercel.app/api/top-langs/?username=hitomi1&layout=compact&langs_count=8&theme=tokyonight&hide_border=true&bg_color=00000000&title_color=7aa2f7">
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
