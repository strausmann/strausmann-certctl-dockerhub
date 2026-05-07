# strausmann-certctl-dockerhub

Dieses Repository stellt eine eigenständige GitHub-Actions-Pipeline bereit, die neue certctl-Releases aus dem Upstream-Projekt automatisch erkennt und als DockerHub-Images veröffentlicht.

## Zweck

Die Pipeline prüft regelmäßig das Upstream-Repository:

- https://github.com/certctl-io/certctl

Wenn ein neuer stabiler Git-Tag gefunden wird, baut und veröffentlicht sie:

- `strausmann/certctl-server` (aus `Dockerfile`)
- `strausmann/certctl-agent` (aus `Dockerfile.agent`)

Der certctl-Quellcode wird dabei nur zur Laufzeit aus dem Upstream-Repository ausgecheckt.

## Benötigte GitHub-Secrets

Folgende Secrets müssen im Repository gesetzt sein:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`

## Tagging-Strategie

Für den neuesten stabilen Upstream-Tag (z. B. `v2.0.71`) werden folgende Tags je Image veröffentlicht:

- `latest`
- `2`
- `2.0`
- `2.0.71`

Beispiel:

- `strausmann/certctl-server:latest`
- `strausmann/certctl-server:2`
- `strausmann/certctl-server:2.0`
- `strausmann/certctl-server:2.0.71`

und analog für `strausmann/certctl-agent`.

## Trigger

Der Workflow `.github/workflows/dockerhub-sync.yml` läuft:

- automatisch 4x täglich (alle 6 Stunden)
- manuell über `workflow_dispatch`

## Duplikatvermeidung

Vor dem Build prüft der Workflow über die DockerHub-API, ob der Zieltag (`<version>`, z. B. `2.0.71`) bereits existiert.

- Existiert `strausmann/certctl-server:<version>`, wird der Server-Build übersprungen.
- Existiert `strausmann/certctl-agent:<version>`, wird der Agent-Build übersprungen.
- Existieren beide bereits, wird kein Build ausgeführt.
