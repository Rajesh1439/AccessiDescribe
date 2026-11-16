# AccessiDescribe

**AccessiDescribe** is a Windows video-accessibility application that uses configurable AI providers to analyze visual scenes and create reviewable spoken audio descriptions, captions, transcripts, and accessible media exports.

It was developed to reduce barriers in video content for blind and low-vision people while also providing useful options for Deaf and hard-of-hearing people, people with cognitive or learning disabilities, keyboard users, and other people with disabilities. AI-generated output can be wrong, so descriptions and captions remain editable and should be reviewed by a person before publication.

> **Release status:** v0.1.0 is the first public preview. The Windows executable is not Authenticode-signed, live provider/model quality has not been certified, and the manual assistive-technology test matrix is incomplete. Do not describe this release as fully accessible, compliant, or production-ready.

## Why AccessiDescribe

Video is central to education, work, public services, and entertainment, but much of its meaning is visual. People who are blind or have low vision can miss actions, settings, on-screen text, facial expressions, and other details that dialogue alone does not convey.

Audio description fills that gap by narrating important visual information during pauses in the soundtrack. Producing it manually takes time, specialist skill, and money, so many videos never receive it. AccessiDescribe reduces that barrier by combining AI scene analysis with an editable, human-review workflow. It also provides captions, transcripts, language options, and caption-presentation controls that can help Deaf and hard-of-hearing users, people with cognitive or learning disabilities, and others who benefit from multiple ways to access media.

AI output can be wrong. AccessiDescribe therefore treats descriptions, captions, and translations as drafts that a person can review, correct, and approve before export.

## How the AI workflow works

1. **Inspect the media.** AccessiDescribe uses FFmpeg and FFprobe to read the video, extract timing information, sample adaptive frames, and analyse the audio for speech and other sound events.
2. **Send only the configured input.** With an OpenAI-compatible provider, selected JPEG frames and prompts are sent. With the dedicated Google Gemini adapter, the user can choose selected frames or full-video upload through the Gemini Files API.
3. **Analyse scenes and audio.** The configured provider returns grounded scene information and, when enabled, timestamped speech recognition and sound-event captions.
4. **Draft descriptions.** The selected language model turns the scene evidence into concise audio-description text in English, Hindi, or Hinglish.
5. **Schedule narration.** AccessiDescribe places descriptions into detected pauses and keeps them away from dialogue and protected audio. A description can be shortened, deferred, kept as transcript-only, or placed on an extended timeline that pauses the picture.
6. **Generate speech.** A provider with `speech_generation` creates narration clips, and the application measures the real clip duration before final placement.
7. **Review and export.** The user edits descriptions and captions, previews the result, and exports accessible video, audio, caption, transcript, and verification files.

## Download

Download the current Windows x64 package from **[GitHub Releases](https://github.com/Rajesh1439/AccessiDescribe/releases/latest)**.

- Package: `AccessiDescribe-Windows-x64.zip`
- Direct v0.1.0 download: [AccessiDescribe-Windows-x64.zip](https://github.com/Rajesh1439/AccessiDescribe/releases/download/v0.1.0/AccessiDescribe-Windows-x64.zip)
- SHA-256: `48A6E8F9C1EF28F026A8C12DBFC611CF2A688481CFAE6806A4F4476622D1CEF9`
- Tested platform: Windows 11 x64

This is a portable ZIP, not an installer. Python, Qt, FFmpeg, and FFprobe runtime components are included.

See **[Installation](INSTALLATION.md)** for extraction, launch, verification, and removal instructions.

## What AccessiDescribe does

AccessiDescribe analyzes a video as a sequence of visual and audio events. Depending on provider configuration, it sends selected JPEG frames or a full video to an AI provider, obtains grounded scene information, writes concise descriptions, converts approved descriptions to speech, and places narration into detected gaps in the original audio.

The standard workflow is designed not to intentionally cover detected dialogue or protected audio. If a description cannot fit safely, the application can shorten it, defer it, retain it in the transcript, or—only when the user enables extended-description mode—pause the original timeline for narration.

## Implemented features

### Video analysis and narration

- Select MP4, MOV, MKV, AVI, or WebM video files.
- Analyze adaptive scene frames with OpenAI-compatible providers.
- Analyze selected frames or upload a full video with the dedicated Google Gemini adapter.
- Extract and analyze audio for timestamped speech and protected-audio intervals.
- Generate objective, evidence-linked scene descriptions.
- Schedule narration around detected dialogue and important audio.
- Generate provider-based text-to-speech narration and measure the real clip duration before final placement.
- Use an optional extended-description export that pauses picture and original audio when a standard gap is not long enough.

### Review and editing

- Processing queue with progress, cancel, and resume controls.
- Audio-description and caption editors with timestamp, placement, language, voice, confidence, and review metadata.
- Regenerate and preview narration for an edited description.
- Add, edit, and delete manual description or caption entries.
- Synchronized video player with original audio, description audio, captions, chapter navigation, playback speed, and volume controls.

### Captions and language

- Timestamped dialogue captions and provider-generated sound-event captions.
- English, Hindi, and Hinglish workflow selections.
- Optional caption translation and simplified caption alternatives.
- Speaker labels, reading-speed checks, text scale, colours, opacity, and top/bottom placement controls.

### Exports

Depending on the completed job, AccessiDescribe can produce:

- mixed MP4 video with audio description;
- MKV video with separate original and description audio tracks;
- MP4 video with an embedded caption track;
- optional extended-description MP4;
- description-only WAV and MP3;
- SRT and styled WebVTT captions;
- transcript and audio-description script text files; and
- synchronization, caption-quality, privacy-disclosure, and export-verification reports.

## Download and installation

1. Open the [AccessiDescribe repository](https://github.com/Rajesh1439/AccessiDescribe).
2. Select **Releases** in the right-hand sidebar, or open the [Releases page](https://github.com/Rajesh1439/AccessiDescribe/releases) directly.
3. Open release **v0.1.0** and download `AccessiDescribe-Windows-x64.zip` from the **Assets** section.
4. Verify the SHA-256 shown above if you require integrity verification.
5. Extract the complete ZIP to a folder you can write to. Do not run the executable from inside the ZIP.
6. Keep `AccessiDescribe.exe` and the `_internal` folder together, then launch `AccessiDescribe.exe`.
7. In AccessiDescribe, open **AI Provider Settings** and add your own Google Gemini API key, the endpoint, and the exact model ID. See [Google Gemini configuration](#google-gemini-configuration) and [AI Provider Configuration](API_CONFIGURATION.md).
8. Open **Upload Video**, select your video, review the cloud-transfer disclosure, and choose the language and description options.
9. Select **Start processing**. Follow progress in **Processing Queue**, then review the generated descriptions and generate or preview spoken narration.
10. Open **Export** to produce the accessible video, audio description, captions, transcript, and verification files you need.

The application starts its local processing service on `http://127.0.0.1:8765` by default. No inbound Internet server is required for normal desktop use.

Because v0.1.0 is unsigned, Windows may display an unknown-publisher or SmartScreen warning. Confirm that the file came from this repository and that its hash matches before deciding whether to run it.

## First-use workflow

AccessiDescribe does not contain a shared API key or a preconfigured AI provider.

1. Open **AI Provider Settings**.
2. Add a provider with its adapter, endpoint, exact model ID, your own API key if required, and only the capabilities that provider supports.
3. Select the provider and run **Test selected**. A live capability test may contact the provider and may incur charges.
4. Configure a `speech_generation` provider if you want generated spoken narration. The dedicated Gemini adapter does not implement speech generation.
5. Open **Upload Video**, select a video, choose language/detail/caption options, and review the cloud-transfer disclosure.
6. If cloud processing is needed, explicitly select the consent checkbox. For local-only processing, configure local providers and select **Use only local AI providers**.
7. Select **Start processing** and monitor **Processing Queue**.
8. Review generated descriptions and captions in the editors.
9. Open **Export** to download the required accessible output files.

Detailed provider instructions are in **[AI Provider Configuration](API_CONFIGURATION.md)**.

## Google Gemini configuration

Obtain your own Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey). Never paste a personal key into an issue, screenshot, log, README, or public repository.

The implementation supports two Gemini configuration paths:

1. **Gemini through Google’s OpenAI-compatible endpoint**
   - Adapter: **OpenAI-compatible**
   - Base URL: `https://generativelanguage.googleapis.com/v1beta/openai/`
   - Video mode: selected adaptive frames

2. **Dedicated Google Gemini adapter**
   - Adapter: **Google Gemini**
   - Base URL: `https://generativelanguage.googleapis.com`
   - Video mode: selected frames or full video upload

This release does **not** hard-code or certify a list of model names. Enter the exact current model ID returned by the configured provider and test each required capability. No Gemini model/key combination was live-certified for v0.1.0, so this documentation intentionally does not invent a model compatibility claim.

### Configure Gemini inside AccessiDescribe

1. Launch AccessiDescribe and open **AI Provider Settings**.
2. Enter a descriptive **Name**, such as `Google Gemini (OpenAI compatible)`.
3. Select the **Adapter**: either **OpenAI-compatible** or **Google Gemini**, as described above.
4. Enter the matching **Endpoint** exactly. Do not append your API key to the URL.
5. Enter your own **API key**. The field masks typed text.
6. Enter the exact **Default model** ID for your account. If different tasks need different models, use the **Capability model overrides** JSON field.
7. Select only the capabilities the endpoint and model actually support.
8. Select **Add**, select the new row, and run **Test selected**.
9. Run a capability-specific test with a non-sensitive sample before processing important media.

A successful connection or `/models` response proves only that the endpoint is reachable. It does not prove that the model accepts images, video, audio, transcription, or speech-generation requests.

### Replace or remove a key

1. Open **AI Provider Settings** and select the provider row.
2. Enter the replacement in **API key** and select **Update selected**.
3. Clear the visible field and run **Test selected**.
4. Revoke the old key in the provider dashboard.

Leaving the key field blank during an update preserves the stored key. To remove a stored key, delete the provider after confirming credential removal, then recreate it if needed. Removing a key inside AccessiDescribe does not revoke it at the provider.

### Gemini and API troubleshooting

| Symptom | What to check |
|---|---|
| `401` or `403` authentication error | Confirm the key belongs to the configured provider and project; replace the key; check that the API, billing, and project are enabled; do not put the key in the endpoint URL. |
| Invalid or missing model | Copy the exact model ID returned for the same endpoint and account; check overrides for spelling; confirm the model is available in your region and has not been retired. |
| Connection or TLS error | Check the base URL path, Internet access, proxy, DNS, system date/time, and trusted TLS certificates. Do not disable certificate verification or use plain HTTP for a cloud endpoint. |
| `429` rate limit or quota | Review provider quota, billing, and request limits. Retrying can incur cost. |
| Model connects but a capability fails | The model may support text only. Disable unsupported capabilities or configure a different provider for images, audio, transcription, or speech generation. |

### Local AI configuration

For a service running on the same computer:

1. Select **Local OpenAI-compatible** as the adapter.
2. Enter the loopback endpoint, for example `http://127.0.0.1:PORT/v1`.
3. Enter the exact model ID loaded by the local service.
4. Leave **API key** blank if the service does not require one.
5. Enable **Restrict this provider to loopback/local processing**.
6. Enable only the routes the service implements, then test each capability.

Many local chat servers do not implement transcription or speech generation. The optional in-process `faster-whisper` mode is implemented in the project but is **not bundled in the v0.1.0 Windows ZIP**; use a separate compatible loopback service for local speech recognition.

See [API_CONFIGURATION.md](API_CONFIGURATION.md) for the full provider matrix, capability overrides, fallback rules, and troubleshooting.

## Privacy and security

- Use only your own provider credentials. Provider secrets are write-only through the application interface and are stored through the Windows credential vault when available.
- Provider keys are not included in this repository or release package.
- Selected-frame mode can transmit extracted JPEG frames and prompts.
- Speech recognition or sound-caption features can transmit extracted audio.
- The dedicated Gemini full-video mode can upload the source or transcoded video through the Gemini Files API.
- Text transformation and narration features can transmit transcript, description, caption, or narration text.
- AccessiDescribe displays a transfer disclosure and requires cloud consent for cloud jobs. The disclosure does not replace the configured provider’s terms, privacy policy, retention policy, or billing rules.
- Local-only mode prevents fallback to cloud providers, but it requires correctly configured local services.
- Do not upload private videos, API keys, provider responses, or unredacted logs to GitHub Issues.

## Accessibility

The Windows interface uses native Qt controls with labels and accessible names, keyboard navigation, menu shortcuts, focus management, status announcements, high-contrast options, configurable text, and non-colour-only status information. Multiline editors allow `Tab` and `Shift+Tab` to move focus rather than trapping keyboard users.

The application also includes caption presentation controls, simplified caption alternatives, transcript reading through Windows speech services, and text-based alternatives to timeline-only editing.

These features are **designed for accessibility**, but the manual NVDA, keyboard-only, high-contrast, large-text, and disabled-user usability matrix has not been completed for v0.1.0. See **[Accessibility](ACCESSIBILITY.md)** for verified behavior and known limits.

## Important limitations

- AI descriptions, transcripts, captions, translations, speaker labels, and sound-event labels can be inaccurate or incomplete. Human review is required.
- A successful provider connection or `/models` response proves reachability, not that a model supports images, full video, audio transcription, speech generation, or every configured language.
- Complete processing normally needs multiple capabilities. Gemini visual analysis alone does not provide text-to-speech in the dedicated adapter.
- Native in-process `faster-whisper` is implemented in the project but is not bundled in the v0.1.0 Windows ZIP. The portable release can use a separate loopback OpenAI-compatible service instead.
- Captioning is not sign-language interpretation.
- The executable is unsigned and the release has not received an independent multi-engine malware or complete binary software-composition assessment.

## Support

### Bug reports and technical support

Use [GitHub Issues](https://github.com/Rajesh1439/AccessiDescribe/issues) and include:

- AccessiDescribe version and Windows version;
- the exact screen and action;
- expected and actual behavior;
- provider adapter and model name, if relevant; and
- a redacted error message.

Never include API keys, authorization headers, private endpoint credentials, private videos, or personal data. The provider diagnostics log records operational stages but should still be reviewed and redacted before sharing.

### Feature requests

Open a [feature request](https://github.com/Rajesh1439/AccessiDescribe/issues/new) describing the accessibility barrier, affected users, and desired outcome. Avoid including private health or identity information.

### Security reports

Do not disclose a suspected vulnerability or credential in a public issue. Use the repository’s **Security** tab and private vulnerability reporting when available.

## Contributions

This repository intentionally distributes documentation and compiled release assets only. Application source code is not published here, so source-code pull requests are not accepted through this repository. Clear bug reports, accessibility feedback, and feature requests are welcome.

## Future improvements

Priorities include code signing, an independent binary security assessment, manual assistive-technology and disabled-user testing, live provider/model compatibility evidence, improved local-AI packaging, and a conventional Windows installer. These are planned areas, not features promised in v0.1.0.

## Documentation

- [Installation](INSTALLATION.md)
- [AI Provider Configuration](API_CONFIGURATION.md)
- [Accessibility](ACCESSIBILITY.md)
- [Troubleshooting](TROUBLESHOOTING.md)
- [Security Policy](SECURITY.md)
