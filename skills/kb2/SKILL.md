---
description: >
  Veredelt RAW-Einträge aus _Wissen/Raw/ zu strukturierten WIKI-Einträgen in _Wissen/Wiki/.
  Clustert RAWs nach Thema, prüft gegen das Fünf-Achsen-Raster aus WIKI-Konventionen,
  schlägt Wiki-Entwurf vor, dedupliziert. Inhaltliche Vault-Konflikte sind KB3-Aufgabe.
  Aufrufbar via /kb2.
---

# KB2 — Veredelungs-Skill (RAW → WIKI)

Vault-Pfad: `C:\AXIS\vault\`

Zweck: alle RAWs mit `reviewed: false` in strukturierte WIKI-Einträge überführen. Form, Struktur, Deduplizierung. Kein inhaltlicher Vault-Abgleich — das ist KB3.

Konventionen: [[WIKI-Konventionen]]. Prüfraster-Definition: dort, Abschnitt „KB2-Prüfraster".

---

## Schritt 1 — RAW-Discovery

Alle Files in `_Wissen/Raw/` mit `reviewed: false` lesen. Nach `domain` gruppieren, innerhalb der Domain nach Tag-Schnittmenge clustern.

Output: Liste der Cluster-Vorschläge — je Cluster: enthaltene RAWs, gemeinsame Tags, Wissenstyp-Vermutung (`korrektur` / `prozess`).

Solo-RAW = Cluster der Größe 1. Kein Zwang zur Gruppierung.

---

## Schritt 2 — Cluster-Freigabe

Cluster-Vorschlag als Fließtext-Übersicht ausgeben. Nutzer bestätigt oder korrigiert Zuschnitt (RAW in anderes Cluster verschieben, Cluster splitten).

wenn Nutzer ablehnt → Skill beenden, nichts schreiben.

---

## Schritt 3 — Prüfbericht pro Cluster

Fünf Achsen aus [[WIKI-Konventionen]] § KB2-Prüfraster:

1. **Integrität** — Frontmatter-Pflichtfelder, Status-Kopplung, File-Naming, Mindest-Tags
2. **Inhaltliche Vollständigkeit** — Tripel bzw. Inhalt/Bezug, TL;DR, summary, quelle
3. **Ubiquitous-Language-Konformität** — alle Fachbegriffe im GLOSSAR
4. **Domänen-/Tag-Plausibilität** — domain-Enum, Inhaltspassung, Modul-Tags
5. **Redundanz / Anreicherung** — Treffer in [[WIKI-Index]] suchen → Anreicherung statt Neueintrag

Befund-Status je Achse: `ok` · `Hinweis` · `Block`.

- **Block** → Cluster gestoppt. Nutzer korrigiert RAW oder skippt Cluster.
- **Hinweis** → erscheint im Bericht, Nutzer entscheidet ob Ergänzung nötig.

**Achse 2 — Lücken-Modus:** Skill schlägt Ergänzung aus dem RAW-Body vor (Umformulierung des Vorhandenen). Keine Ergänzung aus Session-Kontext, kein Erfinden.

**Achse 3 — GLOSSAR-Lookup:** über [[GLOSSAR-Index]], nicht durch Vollladen von GLOSSAR.md. Fehlende Begriffe sammeln.

---

## Schritt 4 — GLOSSAR-Patches

wenn Achse 3 Lücken meldet:
- Skill schlägt je fehlendem Begriff einen GLOSSAR-Eintrag vor (Term + Definition).
- Nutzer geht Liste durch, entscheidet pro Begriff: übernehmen / verwerfen / umformulieren.
- Skill schreibt freigegebene Patches ans **Ende** von `GLOSSAR.md` (nie alphabetisch einfügen — [[CLAUDE]]) und ergänzt `GLOSSAR-Index.md` (Term + Zeilennummer).

---

## Schritt 5 — Wiki-Entwurf

Template: [[Wiki-Template]] aus `_Vorlagen/`.

Pro Cluster ein WIKI-Entwurf:
- File-Name: `WIKI-{thema}.md` (kein Datum — lebendes Dokument)
- Frontmatter: `type: wissen`, `reifegrad: wiki`, `status: entwurf`, `wissenstyp` verbindlich, `quelle` listet alle Source-RAWs als Wikilinks, `tags` aus Cluster-Schnittmenge erweitert
- Body: 
  - `wissenstyp: korrektur` → Annahme / Realität / Grund / Konsequenz
  - `wissenstyp: prozess` → Inhalt / Bezug
- `## Widersprüche`-Abschnitt nur falls bereits im RAW markiert; KB2 erzeugt selbst keine.

**Anreicherung statt Neueintrag:** wenn Achse 5 Treffer in [[WIKI-Index]] liefert → Edit auf bestehendem WIKI: `quelle` erweitern, Body-Diff einarbeiten, `stand` aktualisieren.

---

## Schritt 6 — Vorschau & Freigabe

Vollständiger WIKI-Entwurf (Frontmatter + Body) ausgeben. Bei Anreicherung: Diff gegen bestehendes WIKI.

Nutzer korrigiert oder gibt frei.

wenn Nutzer ablehnt → kein WIKI-File schreiben, kein RAW-Statuswechsel, Cluster überspringen.

---

## Schritt 7 — Schreiben & Statuswechsel

Nach Freigabe:

1. **WIKI-File schreiben** — `_Wissen/Wiki/WIKI-{thema}.md` mit `reifegrad: approved`, `status: aktiv` (Nutzer-Freigabe ist gemäß [[WIKI-Konventionen]] das einzige Gate für diesen Übergang).
2. **Source-RAWs** — je Datei: `reviewed: true`, `status: archiviert`. Inhalt bleibt unverändert (Audit-Spur).
3. **[[WIKI-Index]] ergänzen** — neue Tabellenzeile im passenden `### {domain}`-Abschnitt nach Eintragsformat: `| [[WIKI-thema]] | wissenstyp | approved | tag1, tag2 |`. Abschnitt anlegen wenn noch nicht vorhanden.

---

## Schritt 8 — Nächstes Cluster

Iteration durch alle Cluster aus Schritt 1. Nach letztem Cluster: Skill endet mit Zusammenfassung (n WIKIs erstellt / m angereichert / k übersprungen).

---

## Was der Skill NICHT macht

- **Kein Commit** — [[CLAUDE]]-Verbot. Commit erfolgt durch Cowork-Claude.
- **Kein inhaltlicher Vault-Abgleich** — keine Prüfung gegen ADRs, Specs, Modul-Überdateien. Das ist KB3.
- **Keine Widerspruchs-Auflösung** — KB3-Aufgabe.
- **Kein Re-Capture** — fehlt Substanz in einem RAW, ist das Aufgabe von /capture, nicht /kb2.
- **Kein Edit an freigegebenen WIKIs ohne Achse-5-Treffer** — nur via Anreicherungs-Pfad.

---

## Governance

| Verbot | Quelle |
|---|---|
| Kein Commit durch Skill | [[CLAUDE]] |
| Keine stillen Annahmen | [[CLAUDE]] |
| `GLOSSAR.md` nie direkt laden — immer über [[GLOSSAR-Index]] | [[CLAUDE]] |
| GLOSSAR-Einträge ans Ende anhängen, nie alphabetisch einfügen | [[CLAUDE]] |
| Dokumente mit `status: veraltet` nie laden | [[CLAUDE]] |
| WIKI-File ohne vollständiges Frontmatter ungültig | [[Rulebook_Obsidian]] §2 |
| WIKI-File ohne `quelle` ungültig | [[WIKI-Konventionen]] |
| Provenance-Kette darf nicht abbrechen | [[WIKI-Konventionen]] |
| Reifegrad/Status-Kopplung verbindlich (wiki/entwurf, approved/aktiv) | [[WIKI-Konventionen]] |
| Konfliktprüfung gegen Vault-Regelwerk gehört zu KB3, nicht KB2 | Sprint-Entscheidung 2026-05-26 |
