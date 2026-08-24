<p align="center">
  <em>cs @ usp · I like data engineering and rust, trying to build a startup + HFT</em>
</p>

---

### 👋 About

Most of what I build falls into one of two buckets:

- **Systems that can't be wrong** — event-sourced data, ELT/ETL engines, trading bots. 
- **Tools for games I actually play** — Magic: The Gathering trackers, Dota 2 analyzers. Small scope, real users, shipped.

Mostly  **Python**, **Rust** and **TypeScript**.

---

### 📦 Featured work

#### 🏥 Zela — post-operative patient follow-up `private`

Follow-up platform for surgical teams, built as a pilot for a LASIK, orthopedists and general surgery. Patients answer short structured check-ins on the days their protocol schedules, a deterministic rules engine scores each one 🟢/🟡/🔴, and anything not green lands in a risk-ordered queue where the surgeon reviews it and records whether they agree with the triage.

- **Event-sourced.** Every state change is an immutable row in an append-only `events` table. `UPDATE` and `DELETE` are *revoked* on the application's database role and blocked by a trigger, everything queryable is a projection that `rebuild_all()` can reconstruct from scratch.
- **Protocols are data.** A protocol is a versioned JSON record carrying its check-in questions, schedule, photo requirement, and triage ruleset. Supporting a new specialty means seeding a row. The patient form, the bot's questions, and the surgeon's queue all render from it.
- **Triage is deterministic.** Rules are `{field, op, value}` conditions, the score is the most severe rule that fired. VLM AI in v0.3, when one photo arrives, it may only *escalate* a score.

`FastAPI` · `PostgreSQL` · `React` · `TypeScript` · `Docker Compose` 

#### 📈 polymarket-copy-bot-rs — copy-trading bot `private`

High-performance Polymarket copy-trading bot in Rust. Polls the Data API for a target wallet's fills, mirrors them through the CLOB with EIP-712-signed orders, and tracks positions in migration-managed SQLite. Handles the unglamorous parts that decide whether a bot survives contact with production: on-chain USDC approval at startup, floor-snapping when the wallet is empty, and downgrading expired-market 404s so the logs stay readable. Made a lot of money from this, but it's hard to find good players to copy >.<. It doesn't work properly since Brazil is banned from polymarket and they changed how the API works

`Rust` · `tokio` · `polymarket-client-sdk` · `alloy` · `sqlx` · `reqwest`

#### 🃏 [fdc-tracker](https://github.com/hitomi1/fdc-tracker) — offline MTG draft tracker `public`

Event tracker for the [*Fora da Caixa* MTG Arena community](https://www.instagram.com/foradacaixamtg). Log for Limited results, no server or account required by default, everything lives in `localStorage` and the app is a fully installable offline PWA. Optional Supabase login syncs events across devices. Includes a performance tab with rolling win-rate charts, full reward tables with net-gem math per record, 17Lands import, and a guided first-run tour.

`React 19` · `TypeScript` · `Vite` · `Supabase` · `PWA` · `gh-pages`

#### 🎮 [dotabuff-v2](https://github.com/hitomi1/dotabuff-v2) — live Dota 2 match analyzer `public`

Local web app that identifies all 10 players the moment your match begins: rank, ranked win rate, top heroes, and recent matches. With zero manual input and no browser extension. Dota 2 pushes live state to a local server via **Game State Integration**; player IDs are resolved through a three-tier fallback (Steam `GetRealtimeStats` → STRATZ GraphQL → OpenDota polling), stats are fetched concurrently from OpenDota, and results stream to the browser over Server-Sent Events. You can easily identify the bad players on your team and on the enemy team... Good ones are inexistent.

`Python` · `Flask` · `SSE` · `GSI` · `OpenDota` · `STRATZ`

---

### 🌱 Open source

- **[phase-rs/phase](https://github.com/phase-rs/phase)** — contributed a rules-parser fix to this Rust/WASM Magic: The Gathering engine ([#6540](https://github.com/phase-rs/phase/pull/6540)): scoped `"that player"` to the *triggering* player for Aura and Equipment damage triggers.

---

Made by AI because I’m not capable of talking about myself like that.
