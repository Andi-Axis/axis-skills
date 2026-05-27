---
description: >
  Domänenanforderungen strukturiert ermitteln durch Iteration mit dem 8-köpfigen KI-Key-User-Team
  (A1–A6, T1, T2). Auslösen bei: "Domänenbeschreibung", "Key User Round Table", "KU-Runde",
  "Domäne beschreiben", "Anforderungen ermitteln", "S1 starten", wenn ein Modul oder Sub-Modul
  inhaltlich erarbeitet werden soll, wenn neue Domänenfragen zu klären sind.
  Auch standalone für beliebige Domänenfragen nutzbar — nicht nur als Teil eines vollständigen
  S1-S5-Durchlaufs. Phase S1 im AXIS-Skill-Tree. Ergebnis: aktualisierte Vault-Dokumente.
  Aufrufbar via /domaenenbeschreibung.
---

# S1 — Domänenbeschreibung

Vault-Pfad: `C:\AXIS\vault\`

Zweck: Domänenanforderungen durch strukturierte Iteration mit dem KI-Key-User-Team ermitteln.
Ergebnis: aktualisierte Vault-Dokumente — Modul-/Sub-Modul-Überdateien, GLOSSAR, ggf. ADRs.

---

## Phase 0 — Pre-Check (Pflicht)

Jedes Laden ankündigen: „Ich lade [Dateiname] — [Begründung in einem Satz]."

Pflicht-Loads in dieser Reihenfolge:

1. [[KU-Uebersicht]] — allgemeine Eigenschaften aller KU-Rollen
2. [[GLOSSAR-Index]] — topic-relevante Begriffe identifizieren (**nie** `GLOSSAR.md` direkt laden — [[CLAUDE]])
3. Modul- und Sub-Modul-Überdatei(en) im Topic-Bereich — T1 entscheidet welche geladen werden
4. [[ADR-Index]] — bestehende Architekturentscheidungen im Topic-Bereich prüfen
5. Aktive Specs im Topic-Bereich (`status: entwurf` oder `status: freigegeben`) — T1 entscheidet welche geladen werden

**WIP-Check:** wenn eine geladene Überdatei einen `🔄 S1-WIP`-Marker enthält → nahtlos an Runde N und Topic aus Marker weiterführen.

**Snapshot-Check:** wenn ein Entscheidungs-Snapshot aus einem vorherigen Sub-Modul im Kontext vorliegt → Phase 0 überspringen, Snapshot als Basis verwenden, direkt zu Phase 1.

**Compact-Angebot (Ende Phase 0):**
Nach Abschluss aller Loads — vor Phase 1 — folgende Abfrage ausgeben:

```
Kontext geladen. Möchtest du vor dem Start komprimieren? (empfohlen bei langer Vorgeschichte)
[J] Compact ausführen → dann weiter  |  [N] Direkt starten
```

Nutzer antwortet — bei J: `/compact` ausführen, dann Phase 1 starten. Bei N: direkt Phase 1.

---

## Phase 1 — KU-Auswahl

T1 schlägt anhand der Cross-Module-Übersicht ein KU-Set vor.
Nutzer bestätigt oder ergänzt. Default-Bias: bei Zweifel aufnehmen statt weglassen.

Je aktiviertem KU muss die Einzeldatei geladen werden:

| Kürzel | Datei |
|---|---|
| A1 | [[KU-A1-Vertrieb]] |
| A2 | [[KU-A2-Planung-Disposition]] |
| A3 | [[KU-A3-Beschaffung-Material]] |
| A4 | [[KU-A4-Projekt-QGates]] |
| A5 | [[KU-A5-Produktion-Durchsatz]] |
| A6 | [[KU-A6-Qualitaet]] |
| T1 | [[KU-T1-System-Architekt]] |
| T2 | [[KU-T2-Senior-Developer]] |

**Vereinigungsregel:**
- [[KU-Uebersicht]]-Eigenschaften gelten uneingeschränkt für alle aktivierten KUs.
- **`## Unser Prozess`** — definiert die unternehmensreale Prozessrealität dieser Rolle. Überschreibt generische ERP-Annahmen. KU-Aussagen die gegen `## Unser Prozess` verstoßen sind Fehler — sofort korrigieren.
- Einzeldatei-Erweiterungen (Fachwissen-Anker · Erfahrungsnarben · Argumentationsmuster) ergänzen orthogonal.
- Bei Widerspruch zwischen Übersicht und Einzeldatei: Übersicht gewinnt — Stelle als Konflikt markieren.

---

## Phase 2 — Iterations-Diskussion

### Block A — Emergenz-Runde (einmalig)

**Zweck:** Organische Topic-Emergenz — Unbekanntes aufdecken.

- Alle aktivierten KUs beschreiben den Normalfall aus ihrer Rolle — täglich/wöchentlich, was passiert üblicherweise. **Keine Ausnahmen, keine Randszenarien, keine Lückensuche.** Etwas mehr Detail erlaubt — Kontextgrundlage für Block C.
- **T1: stummer Zuhörer.** Keine Topic-Marker, keine Zwischeneingriffe, keine Architektur-Hinweise. Nur Notizen — T1 führt still eine Topic-Rohlist.
- **T2: Zuhör-Modus.** Eingriff ausschließlich bei Tech-Stack-Konflikt — eine Frage: „Geht das auch anders?" Alles weitere → S2.

---

### Block B — T1-Triage (nach Emergenz-Runde)

**Zweck:** Priorisierte Topic-Queue erstellen — Grundlage für Block C.

T1 erstellt direkt im Anschluss an Block A die Topic-Queue. Ausgabe-Format:

```
Topic-Queue:
1. [Topic-Name] — Grund: [ein Satz] — KUs: [Kürzel]
2. [Topic-Name] — Grund: [ein Satz] — KUs: [Kürzel]
...
```

**Priorisierungskriterien — Reihenfolge verbindlich:**

| Priorität | Kriterium | Leitfrage |
|---|---|---|
| 1 | Abhängigkeit | Blockiert dieses Topic andere Topics? |
| 2 | Konzept-Fundament | Definiert es Basis-Begriffe die andere Topics voraussetzen? |
| 3 | Cross-Module-Impact | Berührt es mehrere Module? |
| 4 | Reversibilität | Ist die Entscheidung schwer rückgängig zu machen? |
| 5 | KU-Batching | Topics mit gleichen KUs gruppieren (Effizienz — kein Inhaltskriterium) |

Nutzer bestätigt oder korrigiert die Queue. Erst dann startet Block C.

---

### Block C — Fokus-Sprints

**Zweck:** Ein Topic vollständig abschließen bevor das nächste beginnt.

**Pro Sprint:**
- Nur die in der Queue benannten KUs sprechen; übrige hören zu und melden sich nur bei direkter Betroffenheit.
- Runde 1: Normalfall für dieses Topic.
- Runde 2+: Lückensuche, Konflikt-Synthese, Optionsskizzen.
- Mindestens 1 Runde · maximal 4 Runden je Sprint.
- Nach 4 Runden ohne Einigung → Konflikt-Eskalation (Format: siehe Phase 3).
- Nach Entscheidung (Phase 4): **retroaktive Konsistenzprüfung** — siehe Phase 3.
- Sprint-Abschluss → nächstes Topic aus Queue.

**Allgemeine Regeln:**
- Topic = ein Punkt der eine Nutzer-Entscheidung erfordert.
- Neuer Glossar-Begriff: Definition inline setzen, damit weiterarbeiten — GLOSSAR-Update in Phase 5.
- Rollenkonformität (vollständige Regeln in [[KU-Uebersicht]]): Jeder KU denkt und spricht ausschließlich aus der eigenen Rolle. Kein Ja-Sagen ohne rollenspezifische Validierung.

---

## Phase 3 — Lösungsansätze pro Topic

T1 als Gatekeeper: entscheidet welche 2–3 Ansätze präsentiert werden.
Keine KU-Mehrheitsabstimmung — T1 allein.

**Retroaktive Konsistenzprüfung (Pflicht nach jeder Topic-Entscheidung):**
Nach Verabschiedung eines Topics prüfen: Hat die aktuelle Entscheidung Einfluss auf bereits verabschiedete Topics? Falls ja → betroffene Topics explizit markieren und Anpassung vornehmen bevor weitergearbeitet wird.

Pflichtblock je Ansatz:

```
Topic: {Name}

Lösungsansatz {A/B/C}:
- Vorschlag: ...
- Was sieht der Anwender davon? ... (ein Satz in Anwendersprache — Pflichtfeld)
- Cross-Module-Impact:
  - Modul X → Auswirkung Y
  - Modul Z → unverändert
- Minderheitsvotum {KU-Kürzel}: ... (nur wenn vorhanden)
```

**Konflikt-Eskalation** (nach 4 Runden ohne Einigung):

```
Konflikt-Eskalation Topic: {Name}
- Konfliktpunkte: ...
- Positionen je KU: {Kürzel}: ...
- T1-Analyse: ...
- Erforderlich: Nutzer-Entscheidung
```

---

## Phase 4 — Nutzer-Entscheidung

Nutzer wählt Lösungsansatz oder gibt Override.
Bei Eskalation: Nutzer entscheidet direkt.
Keine Umsetzung ohne explizite Nutzer-Bestätigung.

---

## Phase 5 — Propagations-Phase (Vault-Update)

Sequenz fix — Reihenfolge nicht variieren:

1. **GLOSSAR.md** — neue Begriffe AM ENDE anhängen (nie alphabetisch einfügen). Workflow → [[CLAUDE]]
2. **GLOSSAR-Index.md** — Term + Zeilennummer ergänzen
3. **Primär-Doku** — Modul- oder Sub-Modul-Überdatei aktualisieren
4. **Sekundär-Doku** — weitere Modul-Überdateien bei Cross-Module-Impact · [[AXIS-Session-Start]] bei neuer Modul-Navigation
5. **ADR + ADR-Index.md** — falls Architekturentscheidung getroffen wurde ([[Rulebook_Obsidian]] Abschnitt 3.5)
6. **`/wikilink-check` aufrufen** — Validierungs-Endschritt

**Eskalationsregeln:**

| Situation | Regel |
|---|---|
| Neues Modul | Höchste Eskalation — Nutzer-Bestätigung Pflicht vor Anlage. T1 schlägt Präfix, Ordner-Nummer und [[AXIS-Session-Start]]-Eintrag vor. |
| Neue Domäne ohne neues Modul | Datei mit `status: entwurf` anlegen |
| Neues Vault-Dokument | Rulebook-Konformität Pflicht: Frontmatter · Wikilinks · Status · Template als Basis ([[Rulebook_Obsidian]] Abschnitt 3) |

Commit: nie durch Skill → [[CLAUDE]]

**Entscheidungs-Snapshot (Ende Phase 5):**

Nach jedem abgeschlossenen Sub-Modul folgenden Block ausgeben:

```
── Snapshot Sub-Modul: {Name} ──────────────────────
Entschieden: {Punkt 1} · {Punkt 2} · {Punkt 3}
Vault-Updates: {Dateiliste}
Offen: {WIP-Marker falls vorhanden / — wenn nichts offen}
────────────────────────────────────────────────────
Kontext-Status: {grün · gelb · rot} — ca. {N} weitere Sub-Module möglich.
Weiter mit nächstem Sub-Modul? [J] Nächstes Sub-Modul benennen  |  [N] Session beenden
```

**Kontext-Status-Logik (T1-Einschätzung):**

| Status | Bedeutung |
|---|---|
| 🟢 grün | Problemlos weitermachen |
| 🟡 gelb | 1 weiteres Sub-Modul sicher · danach Compact oder neue Session empfohlen |
| 🔴 rot | Compact jetzt oder neue Session — weitermachen riskiert Kontextverlust |

Bei 🟡 oder 🔴: Compact-Angebot ausgeben vor dem nächsten Sub-Modul.

---

## Phase 6 — Learnings-Update (nach jeder Session)

**Auslöser — proaktiv, nicht signal-abhängig:**
Jede der folgenden Situationen löst automatisch einen Learning-Kandidaten-Check aus — ohne explizites Signal des Nutzers:
- Nutzer korrigiert eine KU-Aussage (faktisch oder inhaltlich)
- Nutzer präzisiert eine KU-Aussage mit unternehmensrealem Detail
- Nutzer widerspricht einer KU-Annahme
- Nutzer ergänzt einen Prozessschritt den die KU ausgelassen hat
- T1 oder T2 revidieren eine Einschätzung aufgrund von Nutzer-Input

**Nicht warten auf:** „wichtiges Learning", explizite Aufforderung, Zusammenfassung am Ende.
**Stattdessen:** nach jeder Nutzer-Intervention prüfen ob ein Learning-Kandidat vorliegt.

**Prüfsequenz je Learning-Kandidat:**

1. **Kriterien-Check** — mindestens eines muss zutreffen:
   - Unternehmensspezifische Abweichung vom generischen ERP-Denken
   - Wiederkehrendes Missverständnis dieser Rolle
   - Prozessgrenze die in der Realität anders liegt als angenommen
   - Vom Nutzer explizit bestätigt

2. **Zeichenlimit-Check** — aktueller `## Learnings`-Block der betroffenen KU < 700 Zeichen?
   - Ja → direkt anhängen
   - Nein → erst konsolidieren (verwandte Punkte zusammenführen → 2 Punkte werden 1), dann anhängen. Kein Löschen.

3. **Eintragen** — KI-kompakt · keine Füllwörter · max. 120 Zeichen je Eintrag:
   ```
   - {KU-Kürzel}: {kompakter Lernpunkt}
   ```
   Beispiel: `- A1: Anfrage ≠ Schnellerfassung. Immer Projektpaket · mehrteilig · Laufzeit Wochen–Monate.`

**Schreib-Ziel:** `vault/_Key-User/KU-{Kürzel}-*.md` → Abschnitt `## Learnings`

Commit: nie durch Skill → [[CLAUDE]]

---

## Unterbrechung & Wiederaufnahme

**Bei Unterbrechung** — sofort ausführen:

1. WIP-Marker in betroffene Vault-Überdatei eintragen:
   - Modul-Überdatei: Abschnitt `## Offene Diskussionen`
   - Sub-Modul-Überdatei: Abschnitt `## Offene Punkte`

   ```
   > 🔄 S1-WIP — unterbrochen YYYY-MM-DD · Stand: Runde N · Letztes Topic: {Name}
   > Wiederaufnahme: S1 starten mit Verweis auf dieses Dokument
   ```

2. Eintrag in [[SPRINT-Aktuell]] als ToDo mit Wikilink auf das markierte Dokument.

**Bei Wiederaufnahme:** Phase 0 erkennt WIP-Marker — nahtlos an Stand des Markers weiterführen.

---

## Governance & Verbote

Regeln werden verlinkt, nicht dupliziert:

| Verbot | Quelle |
|---|---|
| Kein Commit durch Skill | [[CLAUDE]] |
| Keine stillen Annahmen · Eskalationspflicht bei Geschäftsprozess-/Datenkonflikten | [[CLAUDE]] |
| `GLOSSAR.md` nie direkt laden | [[CLAUDE]] |
| Dokumente mit `status: veraltet` nie laden | [[CLAUDE]] |
| Rulebook-Konformität bei allen Vault-Updates | [[Rulebook_Obsidian]] |
