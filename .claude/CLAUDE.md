# Claude Project Context

## Projektübersicht

Dieses Repository stellt eine GitHub Actions-Pipeline bereit, die certctl-Releases automatisch erkennt und als DockerHub-Images veröffentlicht.

- **Kein Anwendungscode** – ausschließlich CI/CD-Workflow
- **Upstream**: https://github.com/certctl-io/certctl
- **DockerHub**: `strausmann/certctl-server`, `strausmann/certctl-agent`

## Dateistruktur

```
.github/
  workflows/
    dockerhub-sync.yml   # Einzige Pipeline-Datei
  copilot-instructions.md
.claude/
  CLAUDE.md              # Diese Datei
README.md
```

## Workflow-Logik

1. Neuesten stabilen Upstream-Tag via `git ls-remote` ermitteln
2. DockerHub-API prüfen, ob Version bereits existiert
3. Bei neuem Tag: Images bauen und mit 4 Tags pushen (`latest`, Major, Minor, Patch)

## Tagging-Strategie

Beispiel für `v2.0.71`:
- `strausmann/certctl-server:latest`
- `strausmann/certctl-server:2`
- `strausmann/certctl-server:2.0`
- `strausmann/certctl-server:2.0.71`

## Wichtige Invarianten

- `set -euo pipefail` in allen Shell-Skripten
- Duplikatschutz darf nicht entfernt werden
- `latest` zeigt immer auf die höchste stabile Semver-Version
- Keine neuen Secrets einführen ohne README-Update

## Secrets

| Secret | Zweck |
|--------|-------|
| `DOCKERHUB_USERNAME` | DockerHub Login |
| `DOCKERHUB_TOKEN` | DockerHub Push-Berechtigung |
