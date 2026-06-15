# StudentOS — A Lightweight Educational Linux Distro for Students, Programmers & Researchers

## ⚠️ Read this first — the honest scope

The original brief asks for things like "a Linux kernel based modular OS with Rust
system services, secure sandboxing, a React/Electron desktop, a local LLM AI
assistant, a package manager, etc." Taken literally, **that is the scope of a
multi-year project built by a team of dozens of engineers** (this is roughly
what teams behind distros like Pop!_OS, elementary OS, or Fedora Silverblue
spend years on, and they still build *on top of* an existing kernel + base
system rather than writing one from scratch).

What is realistic — and **genuinely educational and impressive** for a
student/solo-dev portfolio — is to build **StudentOS as a Linux distribution
respin**:

- You use the **real, existing Linux kernel** (you don't write one).
- You build a **custom ISO** on top of a base distro (Arch via `archiso`, or
  Debian via `live-build`) — this is exactly how Linux Mint, Manjaro,
  SteamOS, Kali, etc. are made.
- You write **your own apps and tools** (the package manager wrapper, the
  notes app, the AI assistant) as real Rust/Tauri/React projects that run
  *inside* that distro.
- "Modular services," "sandboxing," "process isolation," "multi-user" are
  **already provided by the Linux kernel + systemd + Flatpak/bubblewrap** —
  your job is to *configure and present* them well, not reinvent them.

This repo is structured so every requirement in your spec maps to something
real and buildable. The big teaching document is
[`docs/TEACHING_GUIDE_TH.md`](docs/TEACHING_GUIDE_TH.md) — read that for the
step-by-step "how do I actually build this myself" plan.

---

## Requirements → Reality mapping

| Your spec asks for | What StudentOS actually does |
|---|---|
| Linux kernel based | Base: **Arch Linux** (rolling, lightweight, great docs) via `archiso` |
| Modular services | `systemd` units per feature (notes-sync, ai-assistant, etc.) |
| Secure sandbox execution | `bubblewrap` / `firejail` + Flatpak for 3rd-party apps |
| User permissions / multi-user | Standard Linux users/groups + `polkit` rules |
| Process isolation | `systemd-nspawn` containers for risky student code execution |
| Desktop GUI, dark mode, window manager | **GNOME** (or Hyprland for advanced users) with a custom dark "StudentOS" theme |
| Search launcher | GNOME Activities / or `rofi`/`ulauncher` preconfigured |
| Notification center | GNOME Shell built-in, themed |
| Package manager `stud install/remove/update/search` | A **Rust CLI** that wraps `pacman` + `flatpak`, in `tools/stud-pkg/` |
| Notes, flashcards, citations, planner, PDF reader | A **Tauri + React** app suite in `apps/` |
| AI assistant + local LLM | `ollama` (runs local models) + a thin React chat UI calling it over localhost |
| Research assistant / PDF summarization | Same Tauri app, calls local LLM with extracted PDF text |
| SQLite | Used by every app in `apps/` for local storage |

---

## Repo layout

```
studentos/
├── README.md                  ← you are here
├── docs/
│   ├── ARCHITECTURE.md        ← full system architecture
│   ├── TEACHING_GUIDE_TH.md   ← ★ detailed Thai step-by-step learning plan
│   ├── DEPLOYMENT_UTM.md       ← run StudentOS in UTM on macOS
│   └── ROADMAP.md             ← future roadmap
├── tools/
│   └── stud-pkg/              ← `stud` package manager CLI (Rust, real source)
├── apps/
│   └── studentos-notes/       ← Notes/Flashcards/PDF app skeleton (Tauri+React)
├── build/
│   └── archiso/               ← custom ISO build profile
├── desktop/
│   └── theme/                 ← GTK dark theme + branding
└── scripts/
    └── build-iso.sh           ← one-command ISO build
```

## Quick start (on a real Linux box or VM, NOT this sandbox)

```bash
# 1. Build the stud package manager
cd tools/stud-pkg && cargo build --release

# 2. Build the custom ISO (needs Arch Linux host + archiso package)
sudo ./scripts/build-iso.sh

# 3. Boot the resulting .iso in UTM (see docs/DEPLOYMENT_UTM.md)
```
