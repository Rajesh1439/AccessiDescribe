# Troubleshooting

> **Release scope:** the v0.1.0 GitHub release contains the portable Windows x64 application only. Android sections below document development targets and do not apply to that published release.

## FFmpeg is unavailable

Run `ffmpeg -version` and `ffprobe -version`. Install a trusted Windows build or set the two explicit path environment variables. Restart only the AccessiDescribe process after changing environment variables.

The packaged Windows build includes hash-verified FFmpeg/FFprobe binaries. If its health screen reports them missing, verify that the `media-tools` directory remains beside the packaged application internals and reinstall from a verified archive rather than downloading an arbitrary DLL.

## Desktop cannot start the local backend

Confirm that port 8765 is not occupied by another application and that `.env` does not request an insecure non-loopback bind without a token. The desktop starts a loopback backend automatically by default. Disable **Start the local processing service with the desktop application** only when an independently managed backend is running; `ACCESSIDESCRIBE_DESKTOP_NO_BACKEND=1` provides the same behavior for managed deployments.

## Provider test fails

Check endpoint, selected model, capability, TLS certificate, proxy, quota, and billing. A chat-capable model may not accept images/audio. The UI shows redacted errors; consult the provider dashboard for full billing/rate-limit detail. Never paste keys into logs or issue reports.

Provider transfer diagnostics are written to `logs\providers.log` under the configured AccessiDescribe data directory. The bounded, rotating log records operational stages such as `file_read`, `upload_start`, `upload_done`, and `response_received`; it does not record keys, authorization headers, prompts, response bodies, or signed upload URLs. A missing later stage identifies where a request stopped. The processing queue also displays the failed provider phase and redacted error instead of only `waiting_for_provider`.

## Job says no safe narration gap

This is expected for continuous dialogue or dense sound. Choose transcript-only output, edit/shorten the description, move it to a later verified pause, or explicitly enable extended description mode. The application will not force standard narration over speech.

## Android upload stops

Keep the source document available and do not move it while queued. Confirm the persisted SAF permission, network constraints, backend HTTPS certificate, API token, and WorkManager notification permission. Retry resumes where the backend/job supports it; otherwise the client clearly restarts the upload.

For document providers that report file size, WorkManager reopens the SAF stream at the byte offset persisted by the backend. Every chunk has an offset and SHA-256; a response lost after a chunk or after finalization is safe to retry. The upload screen also preserves the same request identifier and settings after a failed WorkManager run, so selecting **Start processing** again resumes that session. Selecting a different video starts a new session.

If Android reports that byte resume is unavailable, the selected document provider did not expose a size. That transfer uses the compatible multipart fallback and may retransmit after a connection failure. Copying the video to a local provider which reports `OpenableColumns.SIZE` enables byte-level resume. Incomplete partial data can be deleted with **Cancel upload and delete partial data**; abandoned partial sessions are removed when the backend starts after `ACCESSIDESCRIBE_UPLOAD_SESSION_RETENTION_HOURS` (168 hours by default).

## Android cannot connect to the Windows backend

For local development, do not enter the Windows LAN address or expose an unauthenticated API. Start an emulator or attach a USB-debug device, accept its authorization prompt, and run `tools\connect_android_windows.ps1`. Check `tools\android-sdk\platform-tools\adb.exe devices -l`; the target state must be `device`, not `offline` or `unauthorized`. The script verifies both the reverse-port rule and a health request made inside Android.

A fresh debug install uses `http://127.0.0.1:8765/`. Existing app data can retain an older URL, so update **Settings** when the script requests it. The reverse rule disappears after a target disconnect/restart and must then be recreated. Cleartext is intentionally unavailable in release builds; non-ADB devices require a trusted HTTPS endpoint and, for non-loopback backend binds, an API token.

## Tab inserts indentation and focus appears stuck on Windows

Current source and rebuilt packages configure all multiline editors so `Tab`/`Shift+Tab` move focus. If an older packaged build inserts a tab character, install a package built after this regression fix. Record the page and control if focus still cannot leave with either key; tables can use arrow keys internally but must remain escapable with Tab.

## Gradle reports a Google Maven TLS handshake failure

First test the exact artifact URL from the error with `curl.exe -I -L`. If it returns 200, retry Gradle; transient connection termination has been observed. Do not disable certificate validation or add an untrusted mirror. The project pins the Gradle distribution checksum, and Android dependencies remain sourced from `google()`/`mavenCentral()`.

## Screen reader does not announce a control

Record the application version, exact page/control, NVDA or TalkBack version, focus method, and expected/actual announcement. Keyboard users can use the text/list editor paths while the issue is investigated. Do not install an NVDA add-on merely to compensate for missing native semantics without reviewing the security and maintenance impact.

## Database recovery

Stop the application and copy the entire data directory before diagnosis. Do not edit SQLite rows manually. Job media and database records are linked; deleting one side can make recovery impossible.
