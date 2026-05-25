# Vault — Gemeinsame Ressourcen

## Pfade

| Pfad | Zweck |
|---|---|
| `C:\Projekte\Axis\vault\` | Vault — alle Projektdokumente |
| `C:\Projekte\Axis\plans\` | Plans — Implementierungspläne |

---

## Vault-Präfixe

Vault-Dateinamen erkennen an diesen Präfixen:

| Präfix | Bereich |
|---|---|
| `GLOB-` | Globale Dokumente |
| `PROJ-` | Projektdokumente |
| `STAMM-` | Stammdaten |
| `ADR-` | Architecture Decision Records |
| `SERIE-` | Serien |
| `MRP-` | Material Requirements Planning |
| `EINK-` | Einkauf |
| `LAG-` | Lager |
| `VERT-` | Vertrieb |
| `BM-` | Betriebsmittel |
| `QM-` | Qualitätsmanagement |
| `BDE-` | Betriebsdatenerfassung |
| `DASH-` | Dashboards |
| `KU-` | Kundenaufträge |
| `GLOSSAR` | Glossar-Dokumente |
| `SPRINT-` | Sprint-Dokumente |
| `README` | Readme-Dateien |
| `Rulebook` | Regelwerk |
| `Skill-` | Skill-Definitionen |

---

## Verstoß-Typen

| Typ | Bedeutung | Bereinigbar |
|---|---|---|
| `[FREITEXT]` | Freitext-Verweis statt Wikilink | Ja — automatisch |
| `[KEIN-LINK]` | Fehlender Wikilink in Pflichtstruktur | Ja — automatisch |
| `[TABELLE]` | Tabellenzeile ohne Wikilink (Sub-Module / Specs) | Ja — automatisch |
| `[INDEX]` | Dokument fehlt in übergeordnetem Index / Überdatei | Ja — mit Ankündigung |
| `[GLOSSAR-LÜCKE]` | Code-Span oder Kürzel nicht im GLOSSAR-Index | Nein — manuell |
| `[RÜCKLINK-FEHLT]` | Verlinktes Strukturdokument referenziert dieses nicht zurück | Ja — mit Ankündigung |

---

## Dokumenttypen (Frontmatter `type`)

| Typ | Indexierungsprüfung |
|---|---|
| `modul-uebersicht` | Sub-Module-Tabelle auf Wikilinks prüfen |
| `submodul-uebersicht` | Specs-Tabelle + Eintrag in Modul-Überdatei |
| `adr` | Eintrag in ADR-Index |
| `glossar` | Terme in GLOSSAR-Index |
| `skill` | Keine |
| `konzept` | Keine |
| `referenz` | Keine |
| `handoff` | Keine |
| `sprint` | Keine |
| `vorlage` | Keine |
| `spec` | Keine |
