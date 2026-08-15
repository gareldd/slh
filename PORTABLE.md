# Portable storage

Release builds resolve the portable root from the directory containing `SLH.exe`. They never derive persistent paths from the current working directory.

```text
SLH-Portable/
  SLH.exe
  data/
    app.db
    accounts/
    instances/
    shared/
    cache/
    downloads/
    java/
    logs/
    backups/
    languages/
  resources/
    languages/
  licenses/
  README.txt
```

`SLH_PORTABLE_ROOT` is honored only in debug and test builds. It exists to test relocation and Unicode paths without changing production path rules.

The folder can move on the same Windows account without changing relative paths. DPAPI-encrypted account tokens may become unreadable on another account or computer; metadata is retained and the account is marked as requiring sign-in.

Rebuilding an existing `release/SLH-Portable` directory updates the executable and bundled resources in place. The packaging script preserves its `data/` directory so development updates keep the portable database, accounts, instances, and settings.

Import staging remains under `data/downloads`; committed instance files use temporary siblings and rename. Launcher logs rotate daily and SLH removes only its own `slh.log.*` files older than 30 days. Runtime cache is not treated as disposable because installed instances can depend on verified shared assets and libraries.
