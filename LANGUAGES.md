# Languages

Each JSON file in `resources/languages/` is one complete launcher language. The portable build also creates `data/languages/`; users can place their own locale files there without changing the application files.

To add a language:

1. Copy `resources/languages/en-US.json` to `data/languages/<language-code>.json`.
2. Change `meta.code` and `meta.name`.
3. Translate the `common`, `navigation`, `titlebar`, `library`, `settings`, and `legacy` values.
4. Restart SLH and select the new language under Settings → General → Language.

The `legacy` section covers older screens and backend status messages. Provider-owned project names and descriptions from Modrinth/CurseForge are intentionally not translated by SLH.
