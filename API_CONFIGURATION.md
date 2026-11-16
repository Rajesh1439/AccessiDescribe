# AI Provider Configuration

AccessiDescribe v0.1.0 does not include an API key, provider account, local model, or default provider. Each user must configure and test their own providers in **AI Provider Settings**.

## Before you begin

A complete spoken-description job can require several independent capabilities:

| Capability | Used for |
|---|---|
| `video_understanding` | Visual scene and event analysis |
| `language` | Description writing, shortening, and plain-language output |
| `speech_recognition` | Timestamped dialogue transcription |
| `speech_generation` | Text-to-speech narration clips |
| `caption_generation` | Non-speech sound-event captions |
| `translation` | Caption or text translation |

One provider can supply multiple capabilities, or different providers can supply each capability. Enable only capabilities implemented by the selected endpoint and model.

A successful connectivity test or `/models` response proves only that AccessiDescribe reached the endpoint. It does not prove that the selected model accepts images, full video, audio, transcription requests, or speech-generation requests. Use a capability-specific live test and a non-sensitive sample before processing important media. Live tests can incur provider charges.

## Add a provider

1. Launch AccessiDescribe.
2. Open **AI Provider Settings** from the left navigation list.
3. Enter a descriptive **Name**.
4. Select the correct **Adapter**.
5. Enter the provider **Endpoint**.
6. Enter the exact **Default model** ID expected by that endpoint.
7. Enter your own **API key** if required.
8. Select only the provider’s verified **Capabilities**.
9. Select **Selected adaptive frames** or, for the dedicated Gemini adapter only, **Full video upload** when appropriate.
10. Select **Add**.
11. Select the new provider in the table and choose **Test selected**.

The API key field masks typed text. Never use `YOUR_API_KEY_HERE` as a real key; it is documentation placeholder text only.

## Google Gemini through the OpenAI-compatible API

Obtain a personal Gemini API key from:

<https://aistudio.google.com/apikey>

Use these values:

| Field | Value |
|---|---|
| Name | A name such as `Google Gemini (OpenAI compatible)` |
| Adapter | **OpenAI-compatible** |
| Endpoint | `https://generativelanguage.googleapis.com/v1beta/openai/` |
| Default model | The exact current Gemini model ID returned for your account |
| API key | Your own Google AI Studio key |
| Video input mode | **Selected adaptive frames** |
| Local-only | Off |

The OpenAI-compatible adapter sends the key as a bearer credential and uses OpenAI-style routes. For visual analysis it sends selected JPEG frames, not the complete video. The implementation can call routes such as `models`, `chat/completions`, `audio/transcriptions`, and `audio/speech`, but Google’s compatibility endpoint may not implement every route or modality. Do not enable speech recognition, speech generation, or another capability unless the current endpoint/model successfully passes that capability.

No personal Gemini key or model was live-certified during the v0.1.0 build. This configuration path is implemented but must be tested with the user’s current account and model.

## Dedicated Google Gemini adapter

The application also contains a native Gemini adapter with Gemini Files API support.

| Field | Value |
|---|---|
| Adapter | **Google Gemini** |
| Endpoint | `https://generativelanguage.googleapis.com` |
| Default model | The exact current Gemini model ID returned for your account |
| API key | Your own Google AI Studio key |
| Video input mode | **Selected adaptive frames** or **Full video upload** |

The dedicated adapter implements:

- `video_understanding`;
- `language`;
- `speech_recognition`;
- `caption_generation`; and
- `translation`.

It does **not** implement `speech_generation`. Configure another verified provider for text-to-speech narration.

In full-video mode, the source or a processed representation is uploaded through Gemini’s resumable Files API. AccessiDescribe requests deletion of temporary Gemini media after processing. If deletion fails, the job reports a cleanup error so the user can check the provider’s Files dashboard and retention policy.

## Model names supported by v0.1.0

AccessiDescribe intentionally has **no hard-coded model-name allowlist**. Provider model names change, can be account-specific, and can support different modalities under the same API family.

The application accepts the exact non-empty model ID entered by the user and compares it with the endpoint’s model listing when available. Therefore, the truthful supported-model rule is:

> A model is usable only when the configured endpoint returns or accepts that exact ID and a real operation succeeds for each enabled capability.

No Gemini, OpenAI, or local model name is documented as certified for v0.1.0 because no real provider credential/model combination was supplied for release validation. A model appearing in `/models` is not enough to claim image, video, audio, transcription, or TTS support.

If one provider needs different models for different tasks, enter a JSON object in **Capability model overrides**, for example:

```json
{
  "video_understanding": "YOUR_VISUAL_MODEL_ID",
  "speech_recognition": "YOUR_TRANSCRIPTION_MODEL_ID",
  "speech_generation": "YOUR_TTS_MODEL_ID"
}
```

Replace every placeholder with an exact model ID from your provider.

## OpenAI

The dedicated **OpenAI** adapter uses:

```text
https://api.openai.com/v1
```

Enter an OpenAI API key and exact current model IDs for the capabilities you enable. Visual analysis uses selected frames rather than full-video upload. The endpoint and model must separately support the required chat/image, transcription, speech, and audio-caption operations.

No OpenAI model is claimed as live-certified for v0.1.0.

## Other OpenAI-compatible providers

Select **OpenAI-compatible** for an HTTPS service that implements the required OpenAI-style routes. Optional secret headers can be entered as a JSON object in **Secret custom headers**. Header values are write-only and stored with the provider credential.

Do not put credentials in the endpoint URL, model field, provider name, or public configuration. AccessiDescribe rejects endpoint URLs containing user information, query parameters, or fragments.

## Local AI configuration

Select **Local OpenAI-compatible** for a service running on the same computer and listening only on a loopback address.

Example endpoint shape:

```text
http://127.0.0.1:PORT/v1
```

Use the actual port and base path shown by your local service. Then:

1. select **Local OpenAI-compatible**;
2. enter the loopback endpoint;
3. enter the exact model ID loaded by the service;
4. leave **API key** blank if the loopback service does not require one;
5. enable **Restrict this provider to loopback/local processing**;
6. enable only routes the local service implements; and
7. test each capability.

Common route requirements are:

| Capability | Expected compatible route |
|---|---|
| Model discovery | `GET /models` |
| Visual/language/translation | `POST /chat/completions` |
| Speech recognition | `POST /audio/transcriptions` |
| Speech generation | `POST /audio/speech` |

Many local servers implement chat but not transcription or TTS. Configure additional local services/providers when needed.

The codebase has an optional in-process `faster-whisper` mode, but its runtime and models are **not bundled in the v0.1.0 Windows ZIP**. Do not enable **in-process faster-whisper** in this portable release. Use a separate compatible loopback service for local speech recognition.

On **Upload Video**, select **Use only local AI providers** to prevent cloud fallback. A local-only job fails rather than routing media to a cloud provider when no eligible local capability exists.

## Provider priority and fallback

Enabled providers are considered by ascending priority, required capability, local/cloud scope, and circuit-breaker state. Configure more than one verified provider for a capability only when you understand the privacy and billing effect of fallback.

A cloud provider is never eligible for a job marked local-only.

## Replace an API key

1. Open **AI Provider Settings**.
2. Select the provider row.
3. Enter the replacement in **API key**.
4. Select **Update selected**.
5. Clear the visible key field after the update if it has not already cleared.
6. Run **Test selected**.
7. Revoke the old key in the provider dashboard.

Leaving the API key field blank during a normal provider update preserves the existing stored key. To remove a key entirely, remove the provider with **Delete selected** and confirm removal of its stored credentials, then recreate it if needed.

Removing or replacing a key inside AccessiDescribe does not revoke it at the provider. If a key was exposed publicly, revoke it immediately in the provider dashboard and generate a new one.

## Credential storage

- Provider API keys and secret header values are write-only through the provider interface.
- The normal Windows configuration stores them through the operating-system credential vault.
- Provider list responses expose only whether a key exists and the names—not values—of secret headers.
- A blank key on update preserves the existing value.
- Errors are scrubbed for common credential fields and known secret values before display or persistence.

Do not share screenshots while a key is visible in an input field.

## Data sent to providers

What leaves the computer depends on enabled capabilities and video mode:

| Disclosure | Possible transmitted data |
|---|---|
| Local only | Media is sent only to eligible loopback/local providers |
| Frames sent | Selected JPEG frames, timestamps, and prompts |
| Audio sent | Extracted audio for transcription or sound analysis |
| Full video sent | Source or processed video to the dedicated Gemini Files API |
| Text sent | Transcript, caption, description, translation, or narration text |

Cloud jobs require explicit consent in **Upload Video**. Review the provider’s current privacy, retention, regional-processing, training, and billing terms before sending sensitive media.

## Troubleshooting

### Authentication errors (`401` or `403`)

1. Confirm that the key belongs to the configured provider and project.
2. Replace the key using the steps above; do not append it to the endpoint URL.
3. Check whether the provider requires an enabled API, billing account, organization/project selection, or additional secret header.
4. Confirm system date/time and TLS inspection policy.
5. Revoke and regenerate any key that may have been exposed.

Never paste the key into an issue or log.

### Invalid or missing model

1. Copy the exact model ID returned by the same endpoint and account.
2. Remove a `models/` prefix only if the endpoint expects the shorter ID; the dedicated Gemini adapter accepts either form and normalizes it.
3. Check capability-specific model overrides for spelling errors.
4. Confirm that the model is available in the user’s region/project and has not been retired.
5. Run a capability-specific test. Connectivity alone is not sufficient.

### API connection errors

1. Confirm the base URL and required `/v1` or `/v1beta/openai/` path.
2. Confirm Internet access, proxy settings, DNS resolution, and trusted TLS certificates.
3. For local providers, confirm the service is running on the exact loopback port.
4. Check whether another process is using the port.
5. Do not disable TLS verification or change a cloud endpoint to plain HTTP.

### Unsupported capability or invalid response

The model may support text chat but not images, complete video, audio transcription, or speech generation. Disable unsupported capabilities and add another provider for those tasks.

### Rate limit, quota, or billing errors (`429`)

Review the provider dashboard, current quota, billing status, and request limits. Retrying can incur cost. AccessiDescribe’s usage counters are operational indicators and are not billing invoices.

For additional guidance, see [Troubleshooting](TROUBLESHOOTING.md).
