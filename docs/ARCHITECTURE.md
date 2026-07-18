# Architecture

A single self-contained `index.html` holds the entire application: markup, styles, application logic, and cryptography. This document explains how it hangs together and the trade-offs behind the "one file" constraint.

## Design goals

1. **Offline-first** — must work with no network, forever.
2. **Zero infrastructure** — no server, no database, no accounts, no build step.
3. **Portable** — the whole app (and, via export, the whole business) moves as a single file.
4. **Private by default** — data is encrypted on-device and never transmitted.
5. **Beginner-maintainable** — plain web-platform code the owner can actually read and change.

## State &amp; render model

The app is driven by one plain JavaScript state object, `S`:

```
S = {
  profile,      // business identity, money-split %, packages, milestones
  clients[],    // pipeline: leads → booked → delivered → closed (+ payments[])
  ledger[],     // every income/expense line, tagged to a money bucket
  subs[],       // recurring subscription clients + scheduled visit slots
  tasks{}, risks{}, ideas{},
  gig, month, academy, editcourse, ...
}
```

UI is rendered by **pure `render*()` functions** that read `S` and write `innerHTML` into elements addressed by id (`renderAll()` fans out to `renderPipe`, `renderMoney`, `renderCal`, subscriptions, etc.). Any mutation follows the same cycle: **mutate `S` → `save()` → `renderAll()`**. There is no virtual DOM and no framework — just id-addressed render targets, which keeps the whole thing legible and dependency-free.

Tabs are groups of sections. A single `GROUPS` map ties each top-level tab to the section ids it shows, so navigation is one function (`show`).

## Persistence &amp; encryption

Persistence is `localStorage`, but never in plaintext when the platform supports encryption.

```
passcode ──PBKDF2(SHA-256, 310k)──▶ passcode-key ─┐
                                                   ├─(wrap)─▶ master data-key ──AES-GCM──▶ ciphertext ▶ localStorage
recovery code ─PBKDF2─▶ recovery-key ─────────────┘
```

- A random **master data-key** encrypts the state with **AES-GCM 256**.
- That master key is **wrapped twice** — once by a key derived from the passcode, once by a key derived from the one-time recovery code.
- **Passcode reset** therefore only needs to unwrap the master key with the recovery-key and re-wrap it with a new passcode-key — the encrypted business data is never touched or re-encrypted.
- Unlock derives the passcode-key, unwraps the master key, decrypts, and holds the key in memory only for the session. Inactivity triggers auto-lock; repeated failures trigger exponential-backoff lockout.

If the browser lacks Web Crypto (very old/insecure context), the app degrades to a plaintext local mode so it still runs — the security posture is surfaced to the user rather than failing silently.

## Generated documents

Client documents (invoice, contract, receipt, delivery note) are produced by composing the business `profile` with a client record into a print-optimised HTML template (`docShell` / `docHead` + section builders) and opening it for print/PDF. One source of truth (Setup) → consistent, on-brand paperwork with no external templating.

## The `?demo` path

A small, isolated bootstrap (guarded by the `demo` query flag) seeds `S` with a fictional studio, disables persistence, and renders — so a public visitor sees a populated product without a passcode and without writing anything to their browser. It is additive and never runs on the normal path.

## Trade-offs (chosen deliberately)

| Decision | Benefit | Cost |
|---|---|---|
| Single file, no framework | Portable, no build, easy to read | Manual DOM updates; long file |
| `localStorage` per device | Zero infra, fully offline | No automatic multi-device sync (handled via JSON export/import) |
| Client-side-only crypto | True privacy, no backend to breach | Recovery code is the only reset path — lose both and data is unrecoverable (by design) |
| Render-by-id | Tiny, dependency-free | No component reuse niceties of a framework |

These are the right trade-offs for a solo operator who wants control, privacy, and zero running cost — and they make the project a compact demonstration of the raw web platform.
