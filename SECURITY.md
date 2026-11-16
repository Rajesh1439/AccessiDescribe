# Security Policy

## Supported versions

| Version | Support status |
|---|---|
| 0.1.x | Current public preview |

AccessiDescribe v0.1.0 is a portable Windows x64 preview. Security fixes, if required, will be published as a new release.

## Reporting a vulnerability

Please do **not** open a public GitHub issue for a suspected vulnerability or exposed credential.

Use the repository's **Security** tab and select **Report a vulnerability**:

<https://github.com/Rajesh1439/AccessiDescribe/security/advisories/new>

Include, where possible:

- the AccessiDescribe version and Windows version;
- the affected screen or workflow;
- clear reproduction steps;
- the expected and actual behaviour;
- the security impact; and
- redacted diagnostic information.

Never include API keys, authorization headers, private videos, personal data, or unredacted logs in a report. A maintainer will review the report on a best-effort basis and coordinate disclosure after a fix or mitigation is available.

## If an API key is exposed

1. Revoke the key immediately in the provider's dashboard.
2. Generate a replacement key.
3. Update the provider in **AI Provider Settings** inside AccessiDescribe.
4. Check the provider's usage and billing records for unexpected activity.

Deleting a key from a file, a screenshot, or Git history does **not** invalidate it. Revocation at the provider is required.

## Credential handling in AccessiDescribe

- AccessiDescribe does not include a shared API key or a preconfigured provider.
- Provider API keys and secret header values are entered by the user and are write-only through the application interface.
- On a normal Windows installation, provider secrets are stored through the operating-system credential vault using the `keyring` library.
- An explicit encrypted-file fallback is available only when the operator configures a separate credential master key.
- Provider list responses expose whether a key exists, not the key value.
- Diagnostic errors are scrubbed for common credential fields before display or persistence.
- The local backend binds to `127.0.0.1` by default. If it is exposed beyond loopback, an API token and TLS termination at a trusted reverse proxy are required.

For configuration details, see [AI Provider Configuration](API_CONFIGURATION.md).

## Release artifact security

- The v0.1.0 Windows package is distributed as `AccessiDescribe-Windows-x64.zip`.
- `AccessiDescribe.exe` is not Authenticode-signed. Windows may display an unknown-publisher or SmartScreen warning.
- Verify the SHA-256 published on the release page and in [Installation](INSTALLATION.md) before running the application.
- Download only from the official repository: <https://github.com/Rajesh1439/AccessiDescribe/releases>.
- The repository contains documentation and compiled release assets only. It does not contain the application source code.
- Compiled Python applications can be reverse engineered. Obfuscation is not a security boundary.

## Scope

This policy covers the compiled Windows application and the public documentation in this repository. It does not cover third-party AI providers, local AI services, or modified builds supplied by other parties. The project does not operate a bug bounty programme.

Good-faith security research is welcome. Do not access other people's data, disrupt services, or test third-party provider infrastructure without authorization.
