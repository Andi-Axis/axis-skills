---
description: >
  Dokumentiert Session-Learnings als RAW-Eintrag in _Wissen/Raw/.
  Auslösen am Sessionende nach abgeschlossener Arbeit — manuell via /capture.
  Schreibt ein RAW-File pro Session, kein Tripel-Zwang, lieber zu viel als zu wenig.
  Aufrufbar via /capture.
---

# Capture-Skill

Vault-Pfad: `C:\AXIS\vault\`

Zweck: Session-Learnings komprimiert als RAW-File nach `_Wissen/Raw/` schreiben.
Reifegrad `raw` — Rohdokumentation ohne Strukturzwang. Veredelung zu Wiki-Einträgen: Phase 2 (separater Skill).

---

## Schritt 1 — Extraktion

Skill analysiert Session-Verlauf vollständig. Keine Selektion — lieber zu viel als zu wenig.

**Erfassen:**
- Korrekturen: Annahme die durch Unternehmensrealität korrigiert wurde
- Prozesswissen: neu erfahrene Abläufe, Strukturen, Regeln
- Entscheidungen mit Begründung die noch kein Vault-Dokument haben
- Offene Punkte mit Session-übergreifender Relevanz

**Nicht erfassen:**
- Technische Implementierungsdetails ohne Domänenbezug
- Inhalte die bereits vollständig in Vault-Dokumenten stehen (Wikilink reicht)

---

## Schritt 2 — File-Entwurf

Template: `_Vorlagen/Raw-Template.md`

Struktur nach Vorlage ausfüllen:
- **Frontmatter:** title, type, domain, status, stand, reifegrad, strang, wissenstyp, quelle, reviewed, tags, summary
- **Body:** TL;DR + Annahme/Realität/Grund (wissenstyp: korrektur) oder Inhalt/Bezug (wissenstyp: prozess)

**Formulierungsregeln:** [[Rulebook_Obsidian]] §1 und §4 — kein Erklärtext, keine Prosa, jede Zeile trägt Information. Entscheidungspunkte als `wenn X → dann Y`.

**File-Naming:** `RAW-{thema}-{YYYY-MM-DD}.md` — kebab-case, keine Umlaute, gem. [[WIKI-Konventionen]].

---

## Schritt 3 — Vorschau & Freigabe

Vollständigen File-Entwurf (Frontmatter + Body) ausgeben.
Nutzer korrigiert oder gibt frei. Kommunikation: Fließtext.
wenn Nutzer ablehnt → kein File schreiben, Session beenden.

---

## Schritt 4 — Schreiben

Ziel: `_Wissen/Raw/RAW-{thema}-{YYYY-MM-DD}.md`

**Der Skill schreibt nicht:**
- Keinen WIKI-Index-Eintrag — erst wenn RAW → Wiki (Phase 2)
- Keine Vault-Überdatei-Updates
- Kein Sprint-Update

Commit: nie durch Skill → [[CLAUDE]]

---

## Governance

| Verbot | Quelle |
|---|---|
| Kein Commit durch Skill | [[CLAUDE]] |
| Keine stillen Annahmen | [[CLAUDE]] |
| `GLOSSAR.md` nie direkt laden | [[CLAUDE]] |
| Dokumente mit `status: veraltet` nie laden | [[CLAUDE]] |
| RAW-File ohne vollständiges Frontmatter ist ungültig | [[Rulebook_Obsidian]] §2 |
| RAW-File ohne `quelle` ist ungültig | [[WIKI-Konventionen]] |
| Rulebook-Konformität bei RAW-File | [[Rulebook_Obsidian]] §3.8 |
