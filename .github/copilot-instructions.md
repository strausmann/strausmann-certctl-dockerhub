# GitHub Copilot Instructions

## Projektübersicht

Dieses Repository enthält ausschließlich eine GitHub Actions-Pipeline, die certctl-Releases aus dem Upstream-Projekt (`certctl-io/certctl`) erkennt und als Docker-Images auf DockerHub veröffentlicht.

**Kein Anwendungscode** – nur Workflow-YAML und Konfiguration.

## Workflow-Datei

- `.github/workflows/dockerhub-sync.yml` ist die einzige Pipeline-Datei

## Konventionen

- Workflow-Schritte sind in logische Abschnitte gegliedert: Version ermitteln → DockerHub prüfen → Bauen & pushen
- Versionserkennung via `git ls-remote` gegen das Upstream-Repo, gefiltert auf stabile Semver-Tags (`v?X.Y.Z`)
- Duplikatvermeidung: Vor jedem Build wird die DockerHub-API geprüft
- Tags je Image: `latest`, `MAJOR`, `MAJOR.MINOR`, `MAJOR.MINOR.PATCH`
- Images: `strausmann/certctl-server`, `strausmann/certctl-agent`

## Secrets

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`

## Trigger

- Cron: alle 6 Stunden (`0 */6 * * *`)
- Manuell: `workflow_dispatch`

## Hinweise für Copilot

- Änderungen am Workflow sollen die bestehende Duplikatvermeidungslogik nicht brechen
- `latest` soll immer auf die höchste stabile Semver-Version zeigen
- Shell-Skripte im Workflow nutzen `set -euo pipefail`
- Keine Drittanbieter-Actions hinzufügen, die nicht bereits im Workflow verwendet werden
