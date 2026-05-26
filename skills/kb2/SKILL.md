---
description: >
  Veredelt RAW-Einträge aus _Wissen/Raw/ zu strukturierten WIKI-Einträgen in _Wissen/Wiki/.
  Clustert RAWs nach Thema, prüft gegen das Fünf-Achsen-Raster aus WIKI-Konventionen,
  schlägt Wiki-Entwurf vor, dedupliziert. Inhaltliche Vault-Konflikte sind KB3-Aufgabe.
  Aufrufbar via /kb2.
---

# KB2 — Veredelungs-Skill (RAW → WIKI)

> **Zweck:** RAWs mit `reviewed: false` in implementierungstaugliche WIKI-Einträge überführen. Form, Struktur, Detailtiefe, Deduplizierung. Kein inhaltlicher Vault-Abgleich — das ist KB3.

Vault-Pfad: `C:\AXIS\vault\`  
Konventionen: [[WIKI-Konventionen]] — insb. Abschnitt „KB2-Prüfraster".

---

## Schritt 1 — RAW-Discovery

1. Alle Files in `_Wissen/Raw/` mit `reviewed: false` lesen.
2. Nach `domain` gruppieren, innerhalb der Domain nach Tag-Schnittmenge clustern.
3. Solo-RAW = Cluster der Größe 1 — kein Zwang zur Gruppierung.

**Output:** Cluster-Liste — je Cluster: enthaltene RAWs · gemeinsame Tags · Wissenstyp-Vermutung (`korrektur` / `prozess`).

---

## Schritt 2 — Cluster-Freigabe

Cluster-Vorschlag ausgeben. Nutzer bestätigt oder korrigiert (RAW verschieben, Cluster splitten).

wenn Nutzer ablehnt → Skill endet, keine Schreiboperationen.

---

## Schritt 3 — Prüfbericht pro Cluster

Fünf Achsen aus [[WIKI-Konventionen]] § KB2-Prüfraster:

| Achse | Prüfgegenstand | Befund-Status |
|---|---|---|
| 1 Integrität | Frontmatter-Pflichtfelder, Status-Kopplung, File-Naming, Mindest-Tags | `ok` · `Hinweis` · `Block` |
| 2 Vollständigkeit | Detailtiefe implementierungstauglich — siehe § Vollständigkeitsregel | `ok` · `Hinweis` · `Block` |
| 3 Ubiquitous Language | Alle Fachbegriffe im GLOSSAR | `ok` · `Hinweis` · `Block` |
| 4 Domänen-/Tag-Plausibilität | `domain`-Enum korrekt, Modul-Tags passend | `ok` · `Hinweis` · `Block` |
| 5 Redundanz / Anreicherung | Treffer in [[WIKI-Index]] → Anreicherung statt Neueintrag | `ok` · `Hinweis` · `Block` |

**Block** → Cluster gestoppt. Nutzer korrigiert RAW oder überspringt Cluster.  
**Hinweis** → erscheint im Bericht, Nutzer entscheidet.

---

## Vollständigkeitsregel (Achse 2)

**Testkriterium:** Kann ein Entwickler aus dem WIKI-Eintrag allein eine Datenstruktur, einen Prozessschritt oder eine Formblatt-Prüfung ableiten — ohne das RAW zu öffnen?

wenn nein → `Block` mit Hinweis auf fehlende Detailtiefe.

**Verboten:** Dokumentnamen als Wissensersatz.

| ❌ Nicht gut | ✅ Gut |
|---|---|
| `FB-03: Herstellbarkeitsbewertung` (Listenzeile ohne Inhalt) | `FB-03-10 Herstellbarkeitsbewertung — 18 Prüffelder: Maße/Toleranzen, Materialspezifikationen, Oberflächenspez., Funktions-/Leistungsspez., Kapazitätsanforderungen, Messkonzept, Haptik/Optik, besondere Merkmale, Fähigkeitsanforderungen, gesetzliche Anforderungen, CSR, REACH, Kennzeichnung, Verpackung/Logistik, Rückverfolgbarkeit, Prüf-/Fertigungseinreichungen, Produktkenntnis (neu?), Prozesskenntnis (neu?). Ergebnis-Enum: herstellbar / bedingt herstellbar / nicht herstellbar. Unterschriften: Vertrieb, PL, QAP, Produktion, GF.` |
| `Vorkalkulation ist Pflicht vor Projektgenehmigung` | `Vorkalkulation: Stundenaufstellung je Arbeitsbereich (CAD, CAM, AV/Zuschnitt, Fräsen, Drahtschneiden) + Fremdkosten + Zuschlagssatz. Ausgabe: Angebotspreis Min/Norm je Stück. Beispiel BR590: CAD 80h, CAM 95h, Norm-Preis 32.891 EUR.` |
| `Stückliste enthält BG-Gruppen` | `Stücklistenstruktur: BG_Grundaufbau (Platten EN AW 5083) · BG_Anlagen_demontierbar (Anlagen 1–n, Material 1.2099 oder Obomodulan) · BG_Anlagen_TVKL · BG_Normteile (Bohrbuchsen, Auflagescheiben, DPS-Bauteile) · BG_Lehrenwagen · BG_Transportkiste. Teilenummer-Schema: AB{XXXXXX}_{Bauteilname}.` |
| `Konstruktionsabnahme (Blatt 2)` (ohne Inhalt) | `Konstruktionsabnahme FB-11-21 Blatt 2: 4 Prüfbereiche — Rüsten/Teileentsorgung (6 Punkte), Werkzeugelemente (19 Punkte inkl. max. Federweg 70%, Führungssäulen-Entlüftung, Lochstempelauslegung), Arbeitsfolgen/Funktion (5 Punkte), Besonderheiten (4 Punkte).` |
| `Ressourcen: Abteilungen als Planungsressourcen` | `Projektressourcen-Typen: Konstruktion, CAM-Programmierung, AV/Zuschnitt, 3-Achs Fräsen, 5-Achs Fräsen, Drahtschneiden, WZB. Zeitplanung je Arbeitsgang: tr (Rüstzeit) + te (Ausführungszeit) → Summe in Minuten/Stunden auf Arbeitskarte.` |

**Wenn Achse 2 `Block` meldet:**
1. Konkrete Lücke benennen: welche Felder / Strukturen / Werte fehlen im Entwurf.
2. Entsprechende Stelle im RAW-Body zeigen (Zeilennummer oder Abschnittsname).
3. Formulierungsvorschlag aus RAW-Inhalt erstellen — kein Erfinden, kein Session-Kontext.

---

## Schritt 4 — GLOSSAR-Patches

wenn Achse 3 Lücken meldet:
1. Je fehlendem Begriff einen GLOSSAR-Eintrag vorschlagen (Term + Definition).
2. Nutzer entscheidet pro Begriff: übernehmen / verwerfen / umformulieren.
3. Freigegebene Patches ans **Ende** von `GLOSSAR.md` anhängen — nie alphabetisch einfügen.
4. `GLOSSAR-Index.md` ergänzen: Term + neue Zeilennummer.

Laderegel GLOSSAR: [[GLOSSAR-Index]] → Zeilennummer → `Read(GLOSSAR.md, offset=ZEILE, limit=22)`.

---

## Schritt 5 — Wiki-Entwurf

Template: [[Wiki-Template]] aus `_Vorlagen/`.

**Frontmatter:**

| Feld | Wert |
|---|---|
| `type` | `wissen` |
| `reifegrad` | `wiki` |
| `status` | `entwurf` |
| `wissenstyp` | verbindlich aus RAW übernehmen |
| `quelle` | alle Source-RAWs als Wikilinks |
| `tags` | Cluster-Schnittmenge |

**Body je `wissenstyp`:**

- `korrektur` → `## Annahme` · `## Realität` · `## Grund` · `## Konsequenz`
- `prozess` → `## Inhalt` · `## Bezug`

**Inhalt-Pflicht bei `prozess`:** Prozessschritte mit Verantwortlichkeiten · Formblatt-Strukturen mit konkreten Prüfpunkten und Feldern · Kalkulationslogik mit Beispielwerten · Datenstrukturen ableitbar. Prüfkriterium: Vollständigkeitsregel (siehe oben).

**`## Widersprüche`:** nur wenn bereits im RAW markiert — KB2 erzeugt keine eigenen Widersprüche.

**Anreicherung statt Neueintrag:** wenn Achse 5 Treffer in [[WIKI-Index]] → Edit auf bestehendem WIKI: `quelle` erweitern, Body-Diff einarbeiten, `stand` aktualisieren.

---

## Schritt 6 — Vorschau & Freigabe

Vollständiger WIKI-Entwurf ausgeben (Frontmatter + Body). Bei Anreicherung: Diff gegen bestehendes WIKI.

wenn Nutzer ablehnt → kein WIKI-File schreiben, kein RAW-Statuswechsel, Cluster überspringen.

---

## Schritt 7 — Schreiben & Statuswechsel

Nach Nutzer-Freigabe — in dieser Reihenfolge:

1. **WIKI-File schreiben** — `_Wissen/Wiki/WIKI-{thema}.md` mit `reifegrad: approved`, `status: aktiv`.
2. **Source-RAWs** — je Datei: `reviewed: true`, `status: archiviert`. Inhalt unverändert (Audit-Spur).
3. **[[WIKI-Index]] ergänzen** — neue Tabellenzeile im passenden `### {domain}`-Abschnitt: `| [[WIKI-thema]] | wissenstyp | approved | tag1, tag2 |`. Abschnitt anlegen wenn nicht vorhanden.

---

## Schritt 8 — Nächstes Cluster

Iteration durch alle Cluster aus Schritt 1. Nach letztem Cluster: Zusammenfassung (n WIKIs erstellt / m angereichert / k übersprungen).

---

## Was der Skill nicht macht

| Nicht im Scope | Zuständig |
|---|---|
| Commit nach Schreiben | Cowork-Claude (manuell) |
| Inhaltlicher Vault-Abgleich gegen ADRs, Specs | KB3 |
| Widerspruchs-Auflösung | KB3 |
| Substanz ergänzen die im RAW fehlt | /capture |
| Edit an approved WIKIs ohne Achse-5-Treffer | — |
| Dokumentnamen als Wissensersatz akzeptieren | verboten — Block auslösen |

---

## Governance

| Verbot | Quelle |
|---|---|
| Kein Commit durch Skill | [[CLAUDE]] |
| Keine stillen Annahmen | [[CLAUDE]] |
| `GLOSSAR.md` nie direkt laden | [[CLAUDE]] |
| GLOSSAR-Einträge ans Ende, nie alphabetisch | [[CLAUDE]] |
| `status: veraltet` Dokumente nie laden | [[CLAUDE]] |
| WIKI-File ohne vollständiges Frontmatter ungültig | [[Rulebook_Obsidian]] §2 |
| WIKI-File ohne `quelle` ungültig | [[WIKI-Konventionen]] |
| Provenance-Kette darf nicht abbrechen | [[WIKI-Konventionen]] |
| Reifegrad/Status-Kopplung verbindlich | [[WIKI-Konventionen]] |
| KB3-Scope gehört nicht zu KB2 | Sprint-Entscheidung 2026-05-26 |
