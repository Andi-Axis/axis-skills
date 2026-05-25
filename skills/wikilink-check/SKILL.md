---
description: >
  Prüft und bereinigt Vault-Dokumente vor dem Abschluss auf Wikilink-Vollständigkeit,
  Indexierungskonsistenz, GLOSSAR-Abdeckung und Rückverknüpfung. Automatisch auslösen
  wenn der Nutzer ein Vault-Dokument abschließen will — Signalwörter: "freigeben",
  "Freigabe", "Status setzen", "fertig", "abschließen", "umgesetzt",
  "Status auf freigegeben", "Status auf umgesetzt". Auch wenn Claude selbst einen
  Statuswechsel in einem Vault-Dokument vornehmen soll. Nicht aktiv bei reinen
  Leseanfragen, Diskussionen oder Entwurfsarbeiten ohne Abschlussabsicht.
  Aufrufbar via /wikilink-check.
---

# Wikilink-Check

Vault-Pfad: `C:\Projekte\Axis\vault\`

Gemeinsame Ressourcen: `references/vault-resources.md`

Der Skill prüft und bereinigt. Reihenfolge: erst vollständiger Befund, dann Bereinigung
aller mechanisch fixbaren Verstöße, dann Statuswechsel-Entscheidung beim Nutzer.

---

## Schritt 1: Primäres Arbeitsdokument identifizieren

Das Dokument das abgeschlossen werden soll — aus dem Konversationskontext erkennbar.
Wenn unklar: fragen, nicht raten.

Dokument vollständig lesen. Dokumenttyp aus Frontmatter-Feld `type` bestimmen.
Den Dateinamen ohne Pfad und ohne `.md` als `AktuellerDokumentname` merken.

---

## Schritt 2: Wikilink-Prüfung

Alle Querverweise auf andere Vault-Dokumente müssen `[[Dateiname]]`-Wikilinks sein.
Gilt für alle Dokumenttypen.

**Verstoß A — `[FREITEXT]`:**
- `siehe DATEINAME` · `in DATEINAME` · `laut DATEINAME` · `gemäß DATEINAME`
- Relativer Pfad: `../` oder `./` gefolgt von Dateinamen
- `.md`-Suffix ohne vorangehendes `[[` (z.B. `GLOB-Tech-Stack.md`)

Vault-Dateinamen erkennen an Präfixen — vollständige Liste: `references/vault-resources.md`.

**Verstoß B — `[KEIN-LINK]`:**
Abschnitte die Wikilinks vorschreiben:
- `## Struktur`-Feld in Überdateien: Template-Verweise müssen Wikilinks sein
- `## Learnings`: ADR-Verweise müssen Wikilinks sein (`[[ADR-NNN-Titel]]`)

---

## Schritt 3: GLOSSAR-Prüfung (E1)

`[[GLOSSAR-Index]]` ist im Session-Kontext geladen.

Folgende Vorkommen im Dokumentkörper prüfen:
- Code-Spans: `` `Begriff` ``
- Abkürzungen in Klammern: `(FA)`, `(MRP)`, `(BDE)`

Für jeden Term: prüfen ob er im GLOSSAR-Index gelistet ist (Term oder Kürzel).
Nicht gelistet → **`[GLOSSAR-LÜCKE]`**.

Nicht prüfen (kein Domain-Term):
- Programmier-Bezeichner: `null`, `true`, `false`, SQL-Schlüsselwörter (`WHERE`, `IS NULL`)
- Dateinamen: `settings.json`, `CLAUDE.md`
- Technische Feldnamen ohne eigenständige Fachbedeutung: `deletedAt`, `createdAt`, `id`
- HTTP-Verben, Statuswörter, Fehlercodes: `ERR-CUST-001`

Sonderregel Impact-Tabelle (Specs): Entitäten in der `## Impact`-Tabelle sollen als
Code-Span geschrieben sein. E1-Check greift dort normal. Die Impact-Tabelle selbst
erfordert keine Wikilinks — kein `[TABELLE]`-Verstoß dort.

---

## Schritt 4: Indexierungsprüfung

Je nach `type` im Frontmatter prüfen ob das Dokument in übergeordneten Dokumenten
eingetragen ist. Lesevorgänge ankündigen: „Ich lese [Dateiname] zur Indexierungsprüfung."

**`modul-uebersicht`:**
Sub-Module-Tabelle: jedes Sub-Modul muss als `[[Dateiname]]` gelistet sein.
Zeilen ohne Wikilink → **`[TABELLE]`**.

**`submodul-uebersicht`:**
Specs-Tabelle: jede Spec muss als `[[Dateiname]]` gelistet sein. Zeilen ohne Wikilink → **`[TABELLE]`**.
Außerdem: prüfen ob dieses Sub-Modul in der zugehörigen Modul-Überdatei gelistet ist. Fehlt → **`[INDEX]`**.

**`adr`:**
`[[ADR-Index]]` lesen — prüfen ob dieses ADR mit Wikilink gelistet ist. Fehlt → **`[INDEX]`**.

**`glossar`:**
`[[GLOSSAR-Index]]` prüfen — neu angefügte Terme müssen eingetragen sein. Fehlt ein Term → **`[INDEX]`**.

**`skill`, `konzept`, `referenz`, `handoff`, `sprint`, `vorlage`, `spec`:**
Keine Indexierungsprüfung in diesem Schritt.

---

## Schritt 5: Rückverknüpfungs-Prüfung (E4)

Für alle `[[...]]`-Wikilinks im primären Dokument die auf strukturelle Eltern-Dokumente
zeigen (erkennbar an Suffix `-Uebersicht`):

1. Zieldokument lesen.
2. Suchen ob `[[AktuellerDokumentname]]` darin vorkommt.
3. Nicht gefunden → **`[RÜCKLINK-FEHLT]`**.

Nicht prüfen (incidentelle Querverweise):
- `[[GLOSSAR]]`, `[[GLOB-Tech-Stack]]`, `[[GLOB-Skill-Tree]]`, `[[SPRINT-Aktuell]]`
- `[[ADR-NNN-Titel]]` in Learnings
- Bereits durch Schritt 4 abgedeckte Beziehungen

---

## Schritt 6: Befund ausgeben

**Wenn keine Verstöße:**
```
Wikilink-Check: OK — keine Verstöße.
```
→ Statuswechsel kann durchgeführt werden. Warten auf Nutzer-Bestätigung.

**Wenn Verstöße gefunden:**
```
Wikilink-Check: N Verstöße gefunden.

1. [TYP] Zeile N: Beschreibung
2. [TYP] Beschreibung
```

Verstoß-Typen — vollständige Liste: `references/vault-resources.md`.

---

## Schritt 7: Bereinigung

Direkt nach dem Befund — ohne Rückfrage — alle mechanisch fixbaren Verstöße beheben.

**Primäres Dokument (sofort, still):**

`[FREITEXT]`:
- `DATEINAME.md` → `[[DATEINAME]]`
- `siehe DATEINAME` → `[[DATEINAME]]`
- `../Pfad/DATEINAME.md` → `[[DATEINAME]]`

`[KEIN-LINK]`: Plaintext-Verweis → `[[Verweis]]`

`[TABELLE]`: Tabellenzeile `| DATEINAME | ...` → `| [[DATEINAME]] | ...`

**Andere Dokumente (mit Ankündigung):**

Vor jedem Edit: „Ich ergänze [[ZielDokument]]: [was wird ergänzt]."

`[INDEX]` — je nach Dokumenttyp des primären Dokuments:
- `adr`: Neue Zeile in `[[ADR-Index]]`: `| [[AktuellerDokumentname]] | domain | status | Entscheidungssatz aus ## Entscheidung |`
- `submodul-uebersicht`: Neue Zeile in Modul-Überdatei: `| [[AktuellerDokumentname]] | ... |`
- `glossar`: Neuer Eintrag-Term in `[[GLOSSAR-Index]]`: `| [[GLOSSAR#Term\|Term]] | Kürzel | Zeilennummer |`

`[RÜCKLINK-FEHLT]`: In Zieldokument den passenden Abschnitt (Specs-Tabelle, Sub-Module-Tabelle) ergänzen:
Neue Zeile `| [[AktuellerDokumentname]] | status |` — Status aus Frontmatter des primären Dokuments.

**Nicht bereinigbar:**

`[GLOSSAR-LÜCKE]` — bleibt offen. Erfordert neuen GLOSSAR-Eintrag mit vollständiger Definition.
Nach Bereinigung explizit ausgeben: „Offene Punkte (manuell): [Liste der GLOSSAR-LÜCKEN]"

---

## Schritt 8: Abschlussmeldung

```
Bereinigung abgeschlossen: N Verstöße behoben.
Offene Punkte (manuell): [GLOSSAR-LÜCKEN falls vorhanden, sonst: keine]

Statuswechsel bereit — bitte bestätigen.
```

Kein automatischer Statuswechsel. Warten auf Nutzer-Bestätigung.
