---
description: >
  Domänenanforderungen strukturiert ermitteln durch Iteration mit dem 8-köpfigen KI-Key-User-Team
  (A1–A6, T1, T2). Auslösen bei: "Domänenbeschreibung", "Key User Round Table", "KU-Runde",
  "Domäne beschreiben", "Anforderungen ermitteln", "S1 starten", wenn ein Modul oder Sub-Modul
  inhaltlich erarbeitet werden soll, wenn neue Domänenfragen zu klären sind.
  Auch standalone für beliebige Domänenfragen nutzbar — nicht nur als Teil eines vollständigen
  S1-S5-Durchlaufs. Phase S1 im AXIS-Skill-Tree. Ergebnis: aktualisierte Vault-Dokumente.
  Aufrufbar via /s1-domaene.
---

# S1 — Domänenbeschreibung

Vault-Pfad: `C:\AXIS\vault\`

Zweck: Domänenanforderungen durch strukturierte Iteration mit dem KI-Key-User-Team ermitteln.
Ergebnis: Lückenlose Prozessdokumentation der — Modul-/Sub-Modul-Überdateien, GLOSSAR, ggf. ADRs.
Zweck der Modulbeschreibungen - Dient als Lastenheft für die Spec erstellung und Basis des Coding - Agents
---
## Phase 0 — Pre-Check (Pflicht)

Pflicht-Loads in dieser Reihenfolge:

1. [[KU-Uebersicht]] — allgemeine Eigenschaften
2. [[GLOSSAR-Index]] — topic-relevante Begriffe identifizieren (**nie** `GLOSSAR.md` direkt laden — [[CLAUDE]])
3. Modul- und Sub-Modul-Überdatei(en) im Topic-Bereich — T1 entscheidet welche geladen werden
4. [[ADR-Index]] — bestehende Architekturentscheidungen im Topic-Bereich prüfen
5. Aktive Specs im Topic-Bereich (`status: entwurf` oder `status: freigegeben`) — T1 entscheidet welche geladen werden

**WIP-Check:** wenn eine geladene Überdatei einen `🔄 S1-WIP`-Marker enthält → nahtlos an Runde N und Topic aus Marker weiterführen.

---

## Phase 1 — KU-Auswahl

T1 definiert anhand der Cross-Module-Übersicht ein KU-Set 
- Default-Bias: Nur die wichtigen KUs - keine Nebengeräusche

Je aktiviertem KU muss die Einzeldatei geladen werden:

**Vereinigungsregel:**
- [[KU-Uebersicht]]-Eigenschaften gelten uneingeschränkt für alle aktivierten KUs.
- **`## Unser Prozess`** — definiert die unternehmensreale Prozessrealität dieser Rolle. Überschreibt generische ERP-Annahmen. KU-Aussagen die gegen `## Unser Prozess` verstoßen sind Fehler — sofort korrigieren.

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

**Priorisierungskriterien — Reihenfolge verbindlich:**

| Priorität | Kriterium | Leitfrage |
|---|---|---|
| 1 | Abhängigkeit | Blockiert dieses Topic andere Topics? |
| 2 | Konzept-Fundament | Definiert es Basis-Begriffe die andere Topics voraussetzen? |
| 3 | Cross-Module-Impact | Berührt es mehrere Module? |
| 4 | Reversibilität | Ist die Entscheidung schwer rückgängig zu machen? |
| 5 | KU-Batching | Topics mit gleichen KUs gruppieren (Effizienz — kein Inhaltskriterium) |

---

### Block C — Fokus-Sprints

**Ziel:** Vollwertige Iterationsrunden pro Topic vollständig abschließen bevor das nächste beginnt.

**Pro Sprint:**
- Nur die in der Queue benannten KUs blicken auf die Problemstellung, jeder aus seinem Blickwinkel
-Challenge against the glossary - When the user uses a term that conflicts with the existing language, call it out immediately. "Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?"
-When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account' — do you mean the Customer or the User? Those are different things."
-When domain relationships are being discussed, stress-test them with specific scenarios. Invent scenarios that probe edge cases and force the user to be precise about the boundaries between concepts.

Workflow:
- Standardworkflow beschreiben - bei Fragen mehrere Iteartonsrunden mit dem KUs und Nutzer 
- Erarbeite 3 konkrete Lösungvorschläge nach Pareto Prinzip 20% Aufwand = 80% Leistung - wie dies in AXIS abgebildet werden kann.
- Wichtig ist die Einhaltung der in K1-K8 genannten Kriterien
- Prüfen gegen die Krieterien aus Sprint-K-Check
- Sofern Sprint Check bestanden - nächstes Topic

**Sprint-K-Check (vor Sprint-Abschluss — Pflicht):**
T1 prüft nach der letzten Runde. Fehlende Antwort → Nachfrage, Sprint bleibt offen:
- K1/K8: Alle Zustände und erlaubten Übergänge benannt (`Zustand → Zustand`, keine Auslöser/Bedingungen)? Oder explizit: keine Status-Entität?
- K2: Entität klar von verwandten Domänen-Objekten abgegrenzt?
- K3: Kein offener Blocker mit Datenmodell-Relevanz? Alle berührten Shared Entities mit Owner-BC in [[GLOB-Modul-Uebersicht]] §Shared Entities eingetragen?
- K6: Mindestens ein Ausnahmepfad identifiziert — oder explizit als keiner bestätigt?
- K7: Alle Nicht-Status-Regeln als Tabelle `ID · Wenn · Dann`? Regel-IDs vergeben?

**Allgemeine Regeln:**
- Topic = ein Punkt der eine Nutzer-Entscheidung erfordert.
- Neuer Glossar-Begriff: Definition inline setzen, damit weiterarbeiten — GLOSSAR-Update in Phase 5.

Ziel von Phase 2: -Prozessdefinition von jedem Topic lückenlos abschließen

## Phase 3 — Propagations-Phase (Vault-Update)

**Shared-Entity-Pflicht:** Alle Entitäten dieses Sub-Moduls gegen [[GLOB-Modul-Uebersicht]] §Shared Entities prüfen. Entität dort registriert → Owner-BC übernehmen. Entität nicht registriert → in §Shared Entities ergänzen mit Owner-BC. Offener Owner-BC = Blocker — kein Vault-Update bevor Owner-BC geklärt ist.

Sequenz fix — Reihenfolge nicht variieren:

1. **GLOSSAR.md** — neue Begriffe AM ENDE anhängen (nie alphabetisch einfügen). Workflow → [[CLAUDE]]
2. **GLOSSAR-Index.md** — Term + Zeilennummer ergänzen
3. **Primär-Doku** — Modul- oder Sub-Modul-Überdatei aktualisieren. Bei `type: submodul-uebersicht` muss zwingend folgende Struktur angewendet werden:
 ## SUBMODUL-UEBERSICHT-BEFÜLLUNG

Gegen VORLAGE: `_Vorlagen/Submodul-Uebersicht-Template.md` prüfen - noch nicht enthaltene Abschnitte müssen zwingend ergänzt werden.

### Grundregeln

- **Frontmatter `status`**: immer `entwurf` bei Erstanlage
- **Frontmatter `summary`**: 1 Satz · max 200 Zeichen · fachliche Verantwortung — kein Fließtext
- **TL;DR**: max 15 Wörter · kein "Dieses Modul…"-Einstieg
- **Domänen-Objekte**: nur Objekte die dieses Sub-Modul OWNED · fremde Aggregate nur mit `Owned by [[Wikilink]]`
- **Status-Übergänge**: Sektion weglassen wenn kein Status-Feld vorhanden
- **Regeln**: nur aufnehmen wenn nicht trivial aus Status-Übergängen ableitbar
- **Sonderfälle**: nur echte Prozesszweige — kein Fehlerhandling, keine UI-Varianten
- **Offene Punkte**: erledigte Punkte sofort entfernen — kein Done-Status
- **Learnings**: leer lassen bei Erstanlage — Abschnitt trotzdem behalten
- **Specs**: Sektion weglassen wenn kein Spec-Dokument existiert

4. **Sekundär-Doku** — weitere Modul-Überdateien bei Cross-Module-Impact · [[AXIS-Session-Start]] bei neuer Modul-Navigation
5. **ADR + ADR-Index.md** — falls Architekturentscheidung getroffen wurde ([[Rulebook_Obsidian]] Abschnitt 3.5)
6. **`/wikilink-check` aufrufen** — Validierungs-Endschritt

## Phase 4 — Learnings-Update (nach jeder Session)

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

## Governance & Verbote

Regeln werden verlinkt, nicht dupliziert:

| Verbot | Quelle |

| Keine stillen Annahmen · Eskalationspflicht bei Geschäftsprozess-/Datenkonflikten | [[CLAUDE]] |
| `GLOSSAR.md` nie direkt laden | [[CLAUDE]] |
| Dokumente mit `status: veraltet` nie laden | [[CLAUDE]] |
| Rulebook-Konformität bei allen Vault-Updates | [[Rulebook_Obsidian]] |
