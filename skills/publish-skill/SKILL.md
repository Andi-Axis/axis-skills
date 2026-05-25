---
name: publish-skill
description: Veröffentlicht Änderungen am axis-skills Plugin (neue Skills, Updates an bestehenden Skills, Marketplace-Anpassungen). Führt den kompletten Release-Workflow aus: Version-Bump, Commit, Push zu GitHub und Synchronisation in den lokalen Plugin-Cache, sodass die nächste Claude-Code-Session den neuen Stand sieht. Auslösen wenn der Nutzer sagt: "publish skill", "skill veröffentlichen", "axis-skills release", "neuen skill ausrollen", "skill-update verteilen", oder wenn er nach einer Änderung in C:\\Projekte\\Axis-skills\\ um Veröffentlichung bittet. Aufrufbar via /publish-skill.
---

# Publish Skill — axis-skills Release-Workflow

Vollständiger Release-Vorgang für das axis-skills Plugin: vom lokalen Edit bis zur installierten Version in Claude Code.

## Voraussetzungen prüfen

1. Arbeitsverzeichnis ist `C:\\Projekte\\Axis-skills`. Falls nicht: `Set-Location` dorthin.
2. `git status` ausführen. Wenn keine Änderungen: melden "Nichts zu veröffentlichen" und abbrechen.
3. Änderungen kurz anzeigen (`git status --short`). Prüfen ob unerwartete Dateien dabei sind — wenn ja: Nutzer fragen bevor sie mit-committed werden.

## Version-Bump

Sowohl `.claude-plugin/plugin.json` als auch `.claude-plugin/marketplace.json` enthalten die Plugin-Version. Beide müssen identisch sein und bei jeder Veröffentlichung erhöht werden — sonst erkennt der Plugin-Cache die neue Version nicht.

Standard-Bump-Regeln (SemVer, aktuelle Version in plugin.json lesen):
- **Neuer Skill hinzugefügt** → Minor-Bump (z.B. `0.1.0` → `0.2.0`)
- **Änderung an bestehendem Skill / Bugfix** → Patch-Bump (z.B. `0.1.0` → `0.1.1`)
- **Breaking Change** (Skill umbenannt/entfernt, Format-Bruch) → Major-Bump (z.B. `0.1.0` → `1.0.0`)

Im Zweifel mit dem Nutzer absprechen. Wenn klar (z.B. nur ein neuer SKILL.md hinzugekommen): direkt Minor-Bump und im Commit-Message erwähnen.

Beide Dateien bearbeiten:
- `C:\\Projekte\\Axis-skills\\.claude-plugin\\plugin.json` → Feld `version`
- `C:\\Projekte\\Axis-skills\\.claude-plugin\\marketplace.json` → Feld `plugins[0].version`

## Commit-Message generieren

Format:
```
<type>(<scope>): <beschreibung>
```

Types: `feat` (neuer Skill), `fix` (Bugfix), `docs` (Doku-Änderung), `refactor` (Umstrukturierung), `chore` (Build/Wartung).

Beispiele:
- `feat(capture): neuer Skill für Vault-Quick-Capture`
- `fix(wikilink-check): GLOSSAR-Indexprüfung korrigiert`
- `docs(README): Installations-Hinweise erweitert`

## Ausführung

In dieser Reihenfolge:

```powershell
Set-Location C:\\Projekte\\Axis-skills

# 1. Commit + Push
git add -A
git commit -m "<commit-message>"
git push

# 2. Marketplace-Klon aktualisieren (zieht den neuen Stand vom GitHub-Remote)
Set-Location C:\\Users\\a.gaertig\\.claude\\plugins\\marketplaces\\axis-skills
git pull

# 3. Plugin-Cache spiegeln — alte Version-Dir entfernen, neue anlegen
$cacheRoot = "C:\\Users\\a.gaertig\\.claude\\plugins\\cache\\axis-skills\\axis-skills"
$newVersion = (Get-Content C:\\Projekte\\Axis-skills\\.claude-plugin\\plugin.json | ConvertFrom-Json).version
$newCacheDir = "$cacheRoot\\$newVersion"

# Alte Cache-Verzeichnisse (außer dem neuen) löschen
Get-ChildItem $cacheRoot -Directory | Where-Object { $_.Name -ne $newVersion } | Remove-Item -Recurse -Force -ErrorAction Continue

# Neuen Cache anlegen mit Inhalt aus Marketplace-Klon (ohne .git)
New-Item -ItemType Directory -Force -Path $newCacheDir | Out-Null
robocopy C:\\Users\\a.gaertig\\.claude\\plugins\\marketplaces\\axis-skills $newCacheDir /MIR /XD .git .in_use | Out-Null
```

## installed_plugins.json aktualisieren

Datei: `C:\\Users\\a.gaertig\\.claude\\plugins\\installed_plugins.json`

Im Eintrag `axis-skills@axis-skills` anpassen:
- `installPath` → neuer Cache-Pfad mit neuer Version (z.B. `C:\\\\Users\\\\a.gaertig\\\\.claude\\\\plugins\\\\cache\\\\axis-skills\\\\axis-skills\\\\0.2.0`)
- `version` → neue Version
- `lastUpdated` → aktueller ISO-Zeitstempel
- `gitCommitSha` → neuer Commit-Hash (`git rev-parse HEAD` im Repo)

## Abschluss-Meldung

Nach erfolgreicher Ausführung dem Nutzer melden:

```
✅ axis-skills v<neue-version> veröffentlicht
   Commit: <sha>
   Geliefert: <was wurde geändert/ergänzt, kurz>

→ Claude Desktop App neu starten, damit der neue Stand geladen wird.
```

## Fehlerfälle

- **Push schlägt fehl wegen Auth**: Nutzer auf manuellen `git push` im normalen Terminal verweisen (GCM-Browser-Login). Cache-Sync NICHT durchführen, sonst driften Klon und GitHub auseinander.
- **`git pull` im Marketplace-Klon hat Merge-Konflikt**: Nicht erwartbar bei reinem Klon ohne lokale Edits. Falls doch: `git reset --hard origin/main` im Klon.
- **`robocopy` Exit-Code ≥ 8**: Echter Fehler. Cache könnte inkonsistent sein — Nutzer melden, manueller Check.

## Was dieser Skill NICHT tut

- Keine Skill-Inhalte automatisch generieren — nur veröffentlichen
- Kein Linting oder Schema-Check der SKILL.md-Dateien
- Keine Cowork-Synchronisation — Cowork zieht eigenständig per GitHub
