# aibars

Native macOS menu bar app that reads usage from fifteen AI services (Claude,
ChatGPT, Codex, Gemini, Grok, Cursor, GitHub Copilot, and others), using
sessions already open in the browser or files the local tools already write.
Swift and SwiftUI, no dependencies, macOS 13+.

## Repo layout

- `Sources/` — the `aibarsCore` framework: `Auth/` (cookie/session extraction per browser), `Providers/` (per-service usage readers), `Models/`, `Views/`, `Brand/` (logos/marks), `Forecast/`, `History/`, `Local/` (Claude Code local index), `Notifications/`, `System/`.
- `SourcesApp/` — the app target entry point (`aibarsApp.swift`) and entitlements.
- `Tests/aibarsTests/` — unit tests.
- `project.yml` — xcodegen spec; `aibars.xcodeproj` is generated from it and is gitignored.
- `Resources/` — app assets.

## Commands

Setup: `brew install xcodegen`, then `xcodegen generate` (or `make regen`).

- `make build` — regenerate project, then `xcodebuild build`
- `make test` — regenerate project, then `xcodebuild test`
- `make run` — build, then open the built `.app`
- `make open` — regenerate project, then open in Xcode
- `make clean` — clean build artifacts and remove the generated `.xcodeproj`

Before committing: `make test` must pass and `make build` must be clean (no warnings).

## Conventions

- Conventional Commits, imperative mood, summary ≤ 72 chars, one concern per commit, body explains *why* not *what*. `git config commit.template .gitmessage` wires the reminder into your editor.
- Commit scopes are either a provider id (`claude`, `cursor`, ...) or one of `auth`, `brand`, `menu`, `settings`, `models`, `http`, `project`.
- New providers follow the template documented in `README.md#adding-a-new-provider`.
- Full contribution workflow is in `CONTRIBUTING.md`.

## Gotchas

- Current branch is `overhaul`, not `main` — check which branch a change belongs on before committing.
- `aibars.xcodeproj` is generated and gitignored; never hand-edit it, edit `project.yml` and run `xcodegen generate`.
- `Secrets.swift` and `.env`/`.env.local` are gitignored — never commit session tokens, API keys, or other secrets.
- `DEVELOPMENT_TEAM` is blank and code signing is Automatic — local builds don't need an Apple dev account.
- Reads browser cookies (Chrome, Firefox, Safari) for session-based usage lookups; Chromium cookie decryption needs Keychain access and will prompt.
