# Install AccessiDescribe v0.1.0

## System requirements

- Windows 11 x64 (the tested platform)
- Approximately 1 GB of free disk space for the extracted application, plus space for source videos and generated exports
- Internet access when using a cloud AI provider
- A provider account and API key when required by the selected provider

The v0.1.0 download is a **portable Windows ZIP**, not an installer. It includes the application’s Python runtime, Qt libraries, FFmpeg, and FFprobe. Users do not need to install Python or media tools separately.

Other Windows versions and ARM devices have not been validated for this release.

## Download

1. Open <https://github.com/Rajesh1439/AccessiDescribe/releases>.
2. Select **AccessiDescribe v0.1.0**.
3. Under **Assets**, download `AccessiDescribe-Windows-x64.zip`.

Direct download:

<https://github.com/Rajesh1439/AccessiDescribe/releases/download/v0.1.0/AccessiDescribe-Windows-x64.zip>

Do not download similarly named files from third-party sites.

## Verify the download

Expected values:

- Size: `203316318` bytes
- SHA-256: `48A6E8F9C1EF28F026A8C12DBFC611CF2A688481CFAE6806A4F4476622D1CEF9`

In PowerShell, run:

```powershell
Get-FileHash -LiteralPath .\AccessiDescribe-Windows-x64.zip -Algorithm SHA256
```

The complete hash must match the value above. If it does not match, delete the file and download it again from the official release page.

## Extract and launch

### File Explorer

1. Right-click `AccessiDescribe-Windows-x64.zip`.
2. Select **Extract All**.
3. Choose a folder that your Windows account can write to.
4. Open the extracted folder.
5. Run `AccessiDescribe.exe`.

### PowerShell

```powershell
Expand-Archive -LiteralPath .\AccessiDescribe-Windows-x64.zip -DestinationPath .\AccessiDescribe-v0.1.0
.\AccessiDescribe-v0.1.0\AccessiDescribe.exe
```

Do not run the executable directly from inside the ZIP. Keep these items together:

```text
AccessiDescribe.exe
_internal\
```

Moving only the executable will prevent the application from finding required runtime files.

## Windows security notice

`AccessiDescribe.exe` is not Authenticode-signed. Windows may therefore show an unknown-publisher or Microsoft Defender SmartScreen warning. Before deciding whether to run the application:

1. confirm that it was downloaded from the official GitHub release;
2. verify the SHA-256 value; and
3. scan the downloaded ZIP with your maintained security software.

Do not ignore a warning if the source or hash cannot be verified.

## First launch

On a normal desktop configuration, AccessiDescribe starts a local processing service at:

```text
http://127.0.0.1:8765
```

This loopback address is available only on the local computer. The **Home** page shows the backend address, automatic-start setting, and connection test.

No AI provider is included by default. Continue with [AI Provider Configuration](API_CONFIGURATION.md), then:

1. add and test the required AI providers;
2. configure a speech-generation provider if spoken narration is required;
3. open **Upload Video**;
4. select a supported video file;
5. review processing and cloud-consent options; and
6. select **Start processing**.

## Updating

For a future release, download and extract the new ZIP to a new folder rather than copying individual runtime files over an older version. User jobs and settings are stored separately from the portable application folder unless an administrator has explicitly changed the data directory.

## Removal and local data

To remove the portable program, close AccessiDescribe and delete its extracted application folder.

Deleting that folder does **not** necessarily delete jobs, uploaded media, exports, settings, or credentials stored in the Windows user profile. Before removing the application:

1. use **Processing History** to delete jobs and their stored media that are no longer needed;
2. remove provider records that should no longer retain credentials; and
3. follow your organization’s data-retention and secure-deletion policy.

Application data is stored in the current Windows user’s application-data area unless a managed deployment overrides it. Provider secrets use the Windows credential vault when available. Ordinary file deletion cannot guarantee physical erasure on SSDs; use full-disk encryption and an appropriate device-retirement process for sensitive material.

## Problems starting the application

See [Troubleshooting](TROUBLESHOOTING.md). When requesting support, never post API keys, authorization headers, private videos, or unredacted logs.
