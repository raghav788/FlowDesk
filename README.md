# FlowDesk

FlowDesk is a Windows desktop panel that combines Krovi's local-first productivity tools with FlowDesk's local screen capture, recording, and meeting transcription workflow.

## Features

- Krovi-derived tools: clipboard history, timers and Pomodoro, file shelf, quick transfer, media controls, calendar and reminders, weather, notes, explain/read aloud, clocks, conversion, screen time, Circle to Ask, cleanup review, Agent, and Extras.
- FlowDesk capture: area screenshots, screen recording, computer/microphone audio selection, local live Whisper transcription, saved items, transcript reading, and in-panel image/video preview.
- Local library at `Documents\FlowDesk Library`; capture files survive app reinstalls.
- Global area screenshot shortcut: `Ctrl+Alt+S`. Panel shortcut: `Ctrl+Alt+Space` (customizable).
- Windows 10 (build 19041 or newer) and Windows 11 target.

Krovi's existing feature implementations are inherited from the upstream repository. FlowDesk adds the Capture tool and packaging changes. Refer to `docs/product/FEATURE_AUDIT.md` for inherited feature caveats.

## Build the frontend

Install Node.js 20 or later and run `npm ci` then `npm run build`.

## Build the Windows app

Install Rust with the MSVC toolchain, Visual Studio Build Tools with C++ desktop support, .NET 10 SDK, and Python 3.12 x64. Prepare a Python runtime with `faster-whisper==1.2.1`, `av==16.1.0`, `PyAudioWPatch==0.2.12.9`, and their dependencies. Download the CTranslate2 Whisper `base` model as a local model directory. Then run:

```powershell
./scripts/package-windows.ps1 -DotNet 'C:\Program Files\dotnet\dotnet.exe' -PythonRuntime 'C:\path\to\python' -WhisperModel 'C:\path\to\base-model'
npm run tauri -- build
```

The `Windows build` workflow can assemble the speech runtime and Windows installer on a GitHub Windows runner. Start it from the Actions tab; it uploads the NSIS and MSI installers as workflow artifacts.

The packager removes PDB debug symbols, non-English satellite assemblies, and unused PySide6 packages. These are omitted from the source checkout and should not be committed. A packaged app still includes the capture helper, Python speech runtime, and Whisper model; these are large binary assets and account for most of the Windows installer size.

## Privacy

Audio transcription runs on-device. Audio is not sent to a server. Screenshots, recordings, and transcripts are saved under Documents. The inherited AI tools are optional and require the user to provide an AI provider key before making a request.

## Credits and licenses

FlowDesk incorporates the MIT-licensed Krovi project by Naitik Sahu and contributors (`https://github.com/nAItiklearn/krovi`). The capture engine is derived from ShareX and distributed under GPL-3.0; its complete modified source is included in `integrations/CaptureFlow-Source`. Local transcription code is derived from FlowDesk's earlier local application. See `THIRD-PARTY-NOTICES.md` and the included licenses.
