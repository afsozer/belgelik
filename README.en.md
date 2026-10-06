# Belgelik

**Project page:** [avfatihsozer.com/en/projects/belgelik](https://avfatihsozer.com/en/projects/belgelik) · Türkçe: [README.md](README.md)

A personal, self-hosted study environment for legal work: a PDF and video
library, a legislation reader, a study planner and note-taking, synchronized
across multiple devices and offline-first.

Designed for a single user and personal use; there is no app store release,
no account system and no cloud dependency. Data is kept on a server running
on the user's own machine, and devices connect to it over a private network
(Tailscale).

> This repository is public for **review and demonstration** purposes only.
> No license is granted; all rights reserved. Copying, distributing or using
> the code in other projects requires permission.

## Features

**Reading and library**
- Browse the server's PDF library by its folder structure, with full-text
  search across content
- PDF reader: single/double page and vertical scroll layouts, table of
  contents, bookmarks, pen/highlighter annotation (pressure-sensitive
  drawing), night filter
- Your last page, annotations and notes carry over between devices
- Video library: upload, resume where you left off, background playback and
  media controls from the notification

**Legislation**
- Parsing of law texts down to article, paragraph and subparagraph level
  (parser covered by golden-fixture tests)
- On-device offline storage and full-text search (SQLite FTS5/FTS4)
- References to laws/articles in article text become clickable links
- Detection of changed articles on re-import, version comparison
- Per-article notes, highlights and bookmarks

**Study routine**
- Study schedule; resources and laws linked to a course open with a single tap
- Pomodoro timer (runs in the background and via notification), study
  statistics
- Exam countdown

## Architecture

```
 Flutter client                                  Server (Python / FastAPI)
 Android · iOS · macOS · Windows · Linux
 ┌──────────────────────────────────┐             ┌─────────────────────────────┐
 │ Reader / Legislation / Schedule  │             │ REST API, profile isolation │
 │ Local SQLite + FTS cache         │  HTTP/JSON  │ SQLite (per profile)        │
 │ Sync queue (offline)             │ ◄─────────► │ PDF / video store           │
 │ Token: secure storage            │  Tailscale  │ Daily automatic backup      │
 └──────────────────────────────────┘             └─────────────────────────────┘
```

- **Offline-first synchronization:** Changes are written locally first and
  sent to the server from a queue. Conflicts are resolved at record level with
  *last-write-wins*; deletions are carried as tombstones, and the client pulls
  only new changes using a cursor based on the server's receive time.
- **Profile isolation:** Each profile has its own database and library
  folder; all endpoints are authenticated and profile-scoped.
- **Platform-adaptive interface:** A bottom navigation bar on mobile; on
  desktop, a side rail + embedded reader, split view and keyboard shortcuts.
- **Security:** The access token is stored in Android Keystore / iOS–macOS
  Keychain / Windows Credential Store; the server is reachable only from the
  private network.

## Technology

| Layer | Used |
|---|---|
| Client | Flutter / Dart, `pdfrx`, `sqflite` (+ FFI), `media_kit`, `flutter_secure_storage`, `perfect_freehand` |
| Android native | Kotlin (media playback service, notification controls) |
| Server | Python, FastAPI, Uvicorn, SQLite |
| Test / CI | `flutter test`, `pytest`, GitHub Actions (analysis + tests, on every push) |

## Repository structure

```
app/        Flutter client (lib/, platform folders, test/)
server/     FastAPI server (routers/, legislation parser, backup, test/)
contract/   Client–server data contract (JSON schemas)
docs/       Phase-by-phase development notes and design documents
```

## Development process

The project was developed in phases, each defined by its own design document
(`docs/FAZ-*.md`): PDF reader → synchronization → pomodoro and study
schedule → annotation → desktop interface → sync robustness, backups,
tests/CI → legislation module. Design and product decisions belong to the
project owner; the implementation was developed with AI-assisted coding tools
(Claude Code).
