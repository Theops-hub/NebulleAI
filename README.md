# NebulleAI

**One Mac app for coding, agentic work, office documents and chat — running on a model that lives on your Mac.**

NebulleAI does what you would otherwise split between Claude Code, Codex, OpenCode or Pi, a chat window and an office
suite: it edits a code base, runs commands, researches the web, writes and lays out Word, Excel, PowerPoint and PDF
files, and answers questions — in the same conversation, on a local model, with no account to create.

Its intelligence is **Nébulle**: a small dragon who lives on your desktop, or **Living Matter**, a cloud of particles
that takes the shape of what she is doing. Her name comes from the nebulae.

[**Download NebulleAI for Apple Silicon →**](https://github.com/Theops-hub/NebulleAI/releases/latest)

---

## Why another agent

Coding agents are built for the cloud's largest models. A 27-billion-parameter model on a laptop needs a different
harness: shorter, more precise instructions, and tools that measure their own results instead of trusting the model's
word. NebulleAI's orchestrator is built for exactly that.

- **One agent, every task.** Code, shell, files, web research, documents, spreadsheets, slides, PDFs and chat share one
  loop and one conversation. A request is routed to the tools it needs; nothing to switch.
- **Documents that are checked, not just written.** Every PDF it prints is measured page by page — text out of its box,
  overlaps, blank pages, page count against your example — and the agent is told what to fix. A document made "like
  this example" is compared with the example, side by side.
- **Auto mode with a safety net.** Each command is judged before it runs (36 of 36 decisions right on its classifier
  benchmark, no false refusal); risky ones wait for your approval, and the work resumes where it stopped.
- **It keeps its best version.** When a retouch makes a document worse, the agent is told so and can restore the best
  one; at the end of a task the best version is put back.
- **Fast on Apple Silicon.** Two engines ship inside the app: llama.cpp, and Kapitsa — an MLX engine with speculative
  decoding (DFlash2/KFlash). On Qwen 3.6 35B-A3B it writes at a median 85 tokens/s, against 54 with llama.cpp.

## Measured, not promised

Everything below was measured on the developer's MacBook Pro (M1 Max, 32-core GPU, 64 GB, macOS 26.6), October 2026.

### Head-to-head with other harnesses — same model, same server

Aider Polyglot, 30 exercises, one run, all three harnesses on the same Qwen 3.6 35B-A3B (Q6_K) behind one llama.cpp
server, same instructions, same grading:

| Harness | pass@2 | pass@1 |
|---|---|---|
| **NebulleAI** | **20 / 30 (67 %)** | 33 % |
| OpenCode 1.19.0 | 19 / 30 | 27 % |
| Aider 0.86.2 | 19 / 30 | 43 % |

NebulleAI solved the most exercises once it could test and retry; Aider solves more on the first try and sends far fewer
tokens. Claude Code and Codex could not be run against this local model (its chat template), so they are not in the
table.

### Benchmarks of the app itself

| Suite | Cases | Best result |
|---|---|---|
| Documents (Word, Excel, PDF, formats) | 56 | **53 / 56** — Qwen 3.6 35B-A3B · 52 / 56 — Ornith 1.5 9B |
| Real-world requests with automatic checks | 40 | **38 / 40** — Qwen 3.8 27B · 38 / 40 — Ornith 1.5 9B |
| Vision | 56 | **56 / 56** — Qwen 3.8 27B |
| Web research | 30 | **28 / 30** — Qwen 3.6 35B-A3B |
| Agent in a folder | 13 | **12 / 13** — Qwen 3.6 35B-A3B |
| Auto-mode command classifier | 36 decisions | **36 / 36**, 0 false refusals |

Latest document run (Qwen 3.8 27B on Kapitsa): a 3-page sales report with charts, a poster, a CV from a request and a
CV rebuilt from an image template all pass; a 645-song playlist pulled from a paginated API passes 3 / 3. Rebuilding a
long 8-page report from an example is the one case still being tuned.

### 3,548 automated tests

404 test files cover the agent and its auto mode, documents, engines, installation and updates, security, connections
and the companion. The release build runs all of them (24 skipped on machines without a given engine or model), plus 96
animation tests.

### Speed on an M1 Max (median over real tasks)

| Engine · model | Generation | Prompt reading | Requests measured |
|---|---|---|---|
| Kapitsa · Qwen 3.6 35B-A3B (6-bit) | **85 tokens/s** | 713 tokens/s | 583 |
| llama.cpp · Qwen 3.6 35B-A3B (Q6_K) | 54 tokens/s | 657 tokens/s | 8,659 |
| Kapitsa · Qwen 3.8 27B (4-bit) | 45 tokens/s | 128 tokens/s | 1,023 |
| llama.cpp · Ornith 1.5 9B (Q4_K_M) | 39 tokens/s | 445 tokens/s | 1,237 |

## Which model for your Mac

NebulleAI picks the model for your Mac's memory at first launch: the largest one that keeps **at least 128 k tokens of
context** in 4-bit (5-bit for the 9B, which suffers more from 4 bits). Kapitsa ships inside the app; llama.cpp too.

| Mac memory | Recommended model | Engine | Download | Memory at 128 k | macOS gives the GPU |
|---|---|---|---|---|---|
| 16 GB | Ornith 1.5 9B · Q4_K_M | llama.cpp | 6.7 GB | **9.6 GiB** measured | 10.7 GiB |
| 24 GB | Ornith 1.5 9B · Q5_K_M | llama.cpp | 7.6 GB | ≈ 13.7 GiB estimated | 16 GiB |
| 32 GB | Qwen 3.8 27B · 4-bit | Kapitsa | 20.2 GB | **23.8 GiB** measured | 21.3 GiB, raised to 27 GiB on demand |
| 32 GB, GPU memory not raised | Ornith 1.5 9B · Q5_K_M | llama.cpp | 7.6 GB | ≈ 13.7 GiB estimated | 21.3 GiB |
| 48 – 96 GB | Qwen 3.8 27B · 4-bit + DFlash2 drafter | Kapitsa | 20.2 GB | **26 GiB** measured | 36 GiB and up |
| 128 – 192 GB | Ornith 1.5 122B-A10B · Q4_K_M | llama.cpp | 76.8 GB | ≈ 77 – 81 GiB estimated | 96 GiB and up |
| 256 GB | Qwen 3.5 397B-A17B · IQ4_XS | llama.cpp | 190.7 GB | ≈ 184 – 190 GiB estimated | 192 GiB, 217 GiB raised |

"Measured": on the developer's M1 Max, model loaded with 131,072 tokens of context. "Estimated": weights and vision
projector from the published files, KV cache and recurrent state from each model's own GGUF header (Ornith 122B:
3.0 GiB of cache at 128 k, Qwen 3.5 397B: 3.75 GiB; both use attention on one layer in four), plus the engine overhead
measured on Ornith 9B; the upper bound counts the engine's 32 context checkpoints. The 122B and 397B have not been run
by the developer, and their speeds are not measured.

**Raising the GPU memory (32 GB Macs and up).** macOS lets the GPU use about two thirds of a small Mac's memory. With
your one-time approval (administrator password), NebulleAI raises that limit — never above 85 % of the memory, always
leaving 5 GiB to macOS — only while a model that needs it is loaded. It tells you when it does, and gives the memory
back when the model is released, when the app quits, and at restart. Off by default; turning it off removes the helper.

Every model is downloaded from a pinned revision and checked against its published SHA-256 before it is used.

## Install

1. Download `NebulleAI-x.y.z-arm64.dmg` from the [latest release](https://github.com/Theops-hub/NebulleAI/releases/latest).
2. Open it and drag **NebulleAI** to **Applications**.
3. Launch it: it prepares its engines, downloads the model chosen for your Mac, then asks for the macOS permissions it
   needs.

Requires a Mac with Apple Silicon and macOS 15 or later. Updates then arrive through the app's built-in updater; each one
is checked before it is installed (checksums, Apple notarization, a signed release manifest).

## Free, with one paid mode

NebulleAI is free. Its **character mode** needs a paid licence key, tied to your Mac. Key sales are not open yet.

## Privacy, by design

The model runs on your Mac. NebulleAI was built so that what you do with it stays there — and does not pile up there.

- **Encrypted at rest.** Conversation history, the agent's memory, workshop documents and coaching notes are encrypted
  with AES-256-GCM, with keys kept by the macOS Keychain. Erasing destroys the key, so whatever an SSD or a snapshot
  still holds becomes unreadable; these files are also kept out of Time Machine.
- **No files left behind.** Work files go to a private temporary folder that is emptied when the app restarts. Logs are
  stripped of personal content.
- **No telemetry.** The app sends no usage statistics, no crash reports, no analytics. Strict confidentiality is on by
  default.
- **Your network, your choice.** Web research can be set to go out only through your Mac's VPN, or through Tor if you
  run it (SOCKS port 9050 or 9150): if the VPN or Tor does not answer, no search leaves the Mac. This covers web
  research; the Tor route has not yet been tested on a live Tor circuit.
- **Dictation on the device.** Speech is transcribed on your Mac.
- **Your iPhone, without a cloud.** The phone companion reaches your Mac over your own Tailscale network and signs in with
  passkeys (Face ID or Touch ID).

Some features do go online, and only when you use them: updates, model downloads, web research, and the services you
choose to connect.

## The work behind it

NebulleAI is the work of one studio: 678 commits between 6 September and 27 October 2026, about 150,000 lines of
application code, 404 test files, and benchmark suites run again after each change of the orchestrator. Every result on
this page comes from those runs.

## Licence

NebulleAI is proprietary software by KFR studio, free to use under its [licence agreement](LICENSE) (French). The open
source components it contains — llama.cpp, Kapitsa, Electron and others — remain under their own licences, shipped
with the app. This repository holds the installers and updates only, not the source code.

© 2026 KFR studio
