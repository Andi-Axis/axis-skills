# Plans — Verzeichnisstruktur und Dateiformat

## Verzeichnis

`C:\AXIS\plans\`

Das Verzeichnis existiert noch nicht — wird bei Bedarf angelegt.

---

## Dateiname-Konvention

```
PLAN-[Kurzname].md
```

Beispiele:
- `PLAN-Buchungsfluss.md`
- `PLAN-Kundenauftrag-Import.md`
- `PLAN-BDE-Schnittstelle.md`

Kebab-Case, kein Leerzeichen, kein Datum im Namen (Datum kommt ins Frontmatter).

---

## Frontmatter

```yaml
---
title: [Planname]
type: plan
status: offen | in-arbeit | abgeschlossen
erstellt: YYYY-MM-DD
geändert: YYYY-MM-DD
quelle: "[[Quelldokument]]"
---
```

- `quelle`: Wikilink auf die Spec, das ADR oder den Auftrag der den Plan auslöst.
- `status`: Pflichtfeld — immer gesetzt.

---

## Pflichtabschnitte

```markdown
## Ziel
[Ein Satz: was soll umgesetzt werden]

## Kontext
[Vault-Bezug: welche Specs / ADRs / Stammdaten relevant sind]

## Aufgaben
- [ ] Task 1
- [ ] Task 2 (abhängig von Task 1)

## Abhängigkeiten
[Externe Abhängigkeiten: andere Module, Daten, Teams]

## Abnahmekriterien
[Messbare Bedingungen für "abgeschlossen"]
```

---

## Status-Übergänge

```
offen → in-arbeit → abgeschlossen
```

Rückgängig-Übergang (`abgeschlossen → in-arbeit`) nur bei aktivem Rework — explizit kommentieren.
