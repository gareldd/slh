# Account architecture

All providers implement a common account-provider boundary: login, refresh, logout, profile, launch credentials, and skin capabilities. Common metadata is stored in SQLite; secret token material is stored as Windows DPAPI-encrypted blobs keyed by the internal account UUID.

## Offline

Offline identities store no password or token. UUIDs use the standard name-based UUID algorithm over `OfflinePlayer:<username>`. The UI clearly labels the inability to join normal online-mode servers.

## Microsoft

SLH uses the system browser and Authorization Code with PKCE as a public desktop client. It never collects a Microsoft password and never embeds a client secret. Set `SLH_MICROSOFT_CLIENT_ID` in the environment before starting SLH (or when compiling a configured personal build). Without it, the provider remains visible but disabled with setup guidance.

New applications that access the Minecraft: Java Edition game service APIs must also request manual allow-list review through [Minecraft Help](https://help.minecraft.net/hc/en-us/articles/16254801392141). A newly created Entra client ID is therefore necessary but may not be sufficient for Minecraft profile authentication.

The callback listener binds only to `127.0.0.1` on an ephemeral port, validates OAuth `state`, and expires after five minutes. The authorization code is exchanged with its S256 verifier, followed by Xbox user authentication, XSTS for `rp://api.minecraftservices.com/`, and Minecraft Services profile verification. Only the OAuth refresh token and current Minecraft access token are retained, inside the account's DPAPI blob.

## Ely.by

The launcher uses Ely.by's documented launcher authentication endpoint, detects the documented two-factor challenge, and submits a TOTP only when requested. Passwords remain only in the sign-in request and transient UI state; the access token and client token are serialized only into a DPAPI-encrypted account blob.

On the first Ely.by launch, SLH downloads the official authlib-injector 1.2.8 release into the portable cache and accepts it only when its SHA-256 is `9c7f4343e6c82034958ffb48c14a2cb0c85928be7283103ce17da00c6d5a7b10`. The pinned artifact URL and digest can be replaced with `SLH_ELY_AUTHLIB_INJECTOR_URL` and `SLH_ELY_AUTHLIB_INJECTOR_SHA256`, but both overrides must be supplied together. The javaagent is passed as one JVM argument with the documented `=ely.by` service selector.

Diagnostic output must redact authorization headers, access and refresh tokens, auth codes, passwords, and provider credentials.
