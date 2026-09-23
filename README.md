# OmniStream — Standalone Desktop & Media Directory Browser

A fast, beautiful media directory, FTP, and video streaming application that turns open directories, media servers, and native FTP servers into a searchable local library. Built as a standalone desktop application with Electron, with native FTP support and seamless in-app video playback.

---

## Features

- **Native FTP Support** — Direct connection to `ftp://` and `ftps://` servers (anonymous or with user/password credentials). Crawls directories, extracts file sizes and timestamps, and streams media directly.
- **Local Range-Streaming Proxy** — Chromium cannot natively play `ftp://` video streams. OmniStream runs an internal loopback proxy with HTTP `206 Partial Content` Range request support, enabling scrubbing, instant seeking, lazy thumbnail generation, and subtitle support on FTP videos.
- **Fast Concurrent Crawler** — Per-host round-robin scheduling keeps servers saturated while honoring socket limits; supports request timeouts, retries, depth limits, and batched IndexedDB writes.
- **Stop & Resume** — Progress is checkpointed; interrupted crawls can be resumed from where they stopped.
- **Instant Search & Filtering** — The whole library lives in memory with instant search and filter prefixes: `ext:mkv`, `is:series`, `is:movie`, `server:ftp4`.
- **Series Grouping** — Episodes collapse into single folder cards; open to view all episodes.
- **Lazy Thumbnails** — High-performance frame capture as you scroll, cached locally in IndexedDB.
- **Built-in Player** — `player.html` with folder playlist, auto-next, resume position, and keyboard shortcuts.
- **Settings & Server Manager** — Add, remove, and test open directories, FTP mirrors, and Emby/Jellyfin servers.

---

## Desktop Application (macOS, Windows, Linux)

### Run Locally

```bash
# Install dependencies
npm install

# Run the standalone desktop app
npm start

# Run unit & integration tests
npm test
```

### Build & Package Standalone Binaries

```bash
# Package for macOS (DMG & Zip)
npm run dist:mac

# Package for current platform
npm run dist
```

---

## Architecture & Project Structure

| File / Folder | Purpose |
| --- | --- |
| `electron/main.js` | Main process: window management, macOS menu, persistent storage, IPC hub. |
| `electron/ftp-service.js` | Native FTP engine (`basic-ftp`) & local HTTP 206 Range-streaming proxy server. |
| `electron/preload.js` | Preload script: desktop API bridge (`window.electronAPI`) & transparent `chrome.*` compatibility layer. |
| `background.html` / `background.js` | Background crawler service running concurrently with IndexedDB batching and FTP traversal. |
| `shared.js` | Constants, URL normalization (supporting HTTP, HTTPS, and FTP), and IndexedDB helpers. |
| `browser.html` / `browser.js` | Main media library UI with grid/list view, filtering, and instant search. |
| `settings.html` / `settings.js` | Server configuration, FTP server testing, and crawler tuning. |
| `player.html` / `player.js` | Built-in video player with playlist and HLS/DASH/FTP range streaming support. |
| `browser.css` | Premium dark mode design system shared across all views. |
| `test/test-ftp.js` | Automated test suite for FTP parsing, MIME types, and streaming proxy. |
