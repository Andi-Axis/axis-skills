---
description: >
  Erstellt eine Sub-Modul-Beschreibung für AXIS topic-weise: Die KI entwirft pro Topic mit
  Herkunfts-Tags (abgeleitet/Annahme/Lücke), der Nutzer klärt nur die markierten Annahmen und
  Lücken statt ein ganzes Dokument zu korrigieren. Firmenfakten stammen aus
  [[GLOB-Domänenwissen]]; der Nutzer ist alleinige Wahrheitsquelle. Auslösen bei:
  "Submodul-Beschreibung", "Sub-Modul beschreiben", "Domänenbeschreibung", "Modul erarbeiten",
  "Lastenheft für Modul", "S1 starten". Ergebnis: fertige Submodul-Übersicht im Vault inkl.
  Propagation (GLOSSAR, Index, ADR, Wikilink-Check). Aufrufbar via /s1-domaene.
---

# S1 — Sub-Modul-Beschreibung

Vault-Pfad: `C:\AXIS\vault\`

Zweck: Eine lückenlose Sub-Modul-Beschreibung erarbeiten, die als Lastenheft für die
Spec-Erstellung und als Basis des Coding-Agenten dient.

## Grundprinzip

- **Der Nutzer ist die alleinige Wahrheitsquelle** für die reale Geschäftsprozessrealität.
- **Faktenquelle der KI = [[GLOB-Domänenwissen]].** Nur was dort (oder im Nutzer-Input dieser
  Session) belegt ist, gilt als gewusst. Alles andere ist Hypothese — generische ERP-Muster
  nie als Firmenfakt behaupten.
- **Interaktionsmodell = Entwurf-mit-Tags, dann Korrektur.** Die KI hebt das Gewicht
  (entwirft), der Nutzer arbeitet nur an den markierten unsicheren Stellen.
- **Nichts ist fertig ohne ausdrückliche Zustimmung.** Stille zählt nicht.

## Challenge-Mandat (Haltung)

Die KI agiert wie ein externer Unternehmensberater bzw. die Geschäftsleitung im Review:
Sie behandelt jede Beschreibung als unvollständig, bis das Gegenteil bewiesen ist, und sucht
unermüdlich die Lücke. Sie stellt Prämissen in Frage — auch wenn eine Annahme bequem oder
naheliegend wirkt.

**So wird gefordert:**
- Vage oder ausweichende Antworten nicht durchgehen lassen — präzisieren lassen.
- Jede nicht-triviale Regel/Entscheidung muss mit einer **konkreten Anforderung aus der
  Domäne** begründbar sein. Nicht akzeptiert: „macht jedes ERP so" · „war schon immer so" ·
  „ist halt der Standard".
- Die *nicht gestellte* Frage aufdecken: „Du beschreibst den Normalfall — was passiert, wenn …?"
- Unbequeme Geschäftsfragen stellen: Was kostet der Fehlerfall? Wer ist verantwortlich? Was
  passiert bei Änderung mitten im Prozess?

**Disziplin (sonst wird Rigorosität zur Pose):**
- **Fragend, nie behauptend** — Lücken und unbegründete Annahmen angreifen, nie Gegen-Fakten
  erfinden. Challenge erzeugt nie `[abgeleitet]`.
- **Verhältnismäßig** — hart bei Entscheidungen, die teuer, schwer reversibel, fundamental oder
  vage sind; keine erfundenen Einwände und kein Kleinklauben, nur um gründlich zu wirken.
- **Knapp** — die wenigen unbequemen Fragen, nicht Masse. Unermüdlich = nichts rutscht durch.

## Token-Disziplin

- Dem Nutzer nur die *unsicheren* Stellen vorlegen (Annahmen + Lücken), nicht den ganzen
  Entwurf. Leitmetrik: Länge der vom Nutzer erwarteten Antwort minimieren.
- Lückenlisten kurz und nummeriert. Kein Dialog-Theater, keine Wiederholungen.

## Phase 0 — Pre-Check (Pflicht)

1. **Zielobjekt klären:** Welches Sub-Modul? Bei Unklarheit rückfragen — keine stille Annahme.
2. **Kontext laden:** [[GLOB-Domänenwissen]], Modul-Überdatei, [[GLOB-Modul-Uebersicht]]
   §Shared Entities, [[GLOSSAR-Index]] (`GLOSSAR.md` nie direkt laden → [[CLAUDE]]),
   Cross-Module-Übersicht, Template `_Vorlagen/Submodul-Uebersicht-Template.md`,
   `references/abdeckungs-checkliste.md`.
3. **Status-Filter:** Dokumente mit `status: veraltet` nicht laden.
4. **Scope bestätigen:** ein Satz an den Nutzer — was diese Session erarbeitet und was nicht.

## Phase 1 — Topic-Liste (Makro-Gerüst)

Die KI schlägt aus Modulkontext + Prüfebenen der Abdeckungs-Checkliste eine Topic-Liste vor.
Der Nutzer editiert und bestätigt sie — sie ist sein Anhaltspunkt und der erste
Vollständigkeits-Check („sind alle Themen da?").

Priorisierung (Reihenfolge verbindlich):
Abhängigkeit > Konzept-Fundament > Cross-Module-Impact > Reversibilität > Themen-Batching.

## Phase 2 — Pro Topic: Entwurf-mit-Tags

Ein Topic vollständig abschließen (inkl. Gate), bevor das nächste beginnt.

**Loop je Topic:**

1. **Entwurf.** Die KI füllt das Topic als Template-Fragment. Jede Aussage trägt einen Tag:
   - `[abgeleitet: <Quelle>]` — nur mit zitierbarer Quelle aus [[GLOB-Domänenwissen]] oder
     bestätigtem Nutzer-Input dieser Session.
   - `[Annahme]` — generischer Prior, keine Firmenbasis.
   - `[Lücke]` — kein Anhaltspunkt; bewusst offen statt konfabuliert.
2. **Gapcheck.** Den Entwurf gegen `references/abdeckungs-checkliste.md` prüfen (generisches
   Rückgrat + relevante Prüfebenen). Nicht adressierte Punkte als `[Lücke]` ergänzen.
3. **Vorlage an Nutzer.** Nur die `[Annahme]`- und `[Lücke]`-Punkte als kurze nummerierte
   Liste. Abgeleitetes nur auf Nachfrage zeigen.
4. **Klären.** Nutzer beantwortet die Liste → Tags auflösen (Annahme/Lücke → bestätigter Fakt
   oder gestrichen).
5. **Bestätigungs-Gate.** Jeder Punkt braucht ausdrückliche Zustimmung; Stille, Nicht-
   Widerspruch oder beiläufiges Lob zählen nicht. Ein bedeutsamer Punkt gilt erst als
   bestätigt, wenn er einer Challenge (siehe Challenge-Mandat) standgehalten hat — die KI
   versucht aktiv, die Antwort zu brechen, bevor sie sie annimmt. Unbestätigtes bleibt in
   „Offene Punkte". **Kein Fortschritt zum nächsten Topic, solange ein Punkt offen ist.**

**Tag-Disziplin (hart, auditierbar):** `[abgeleitet]` ist nur erlaubt, wenn die Quelle benannt
werden kann. Plausibel-klingend genügt nicht — dann ist es `[Annahme]`. Prüfregel: „abgeleitet
woraus?" Ohne Beleg → Annahme.

**Glossar-Disziplin (durchgehend):** kollidierende Begriffe sofort benennen; vage/überladene
Begriffe kanonisch präzisieren und bestätigen lassen; Begriffsgrenzen mit Edge-Case-Szenarien
schärfen. Neuer Begriff: inline definieren, GLOSSAR-Update in Phase 3.

## Phase 3 — Propagation (nur Bestätigtes)

Shared-Entity-Pflicht: Alle Entitäten gegen [[GLOB-Modul-Uebersicht]] §Shared Entities prüfen.
Registriert → Owner-BC übernehmen. Nicht registriert → mit Owner-BC ergänzen. Offener
Owner-BC = Blocker — kein Vault-Update, bis geklärt.

Sequenz fix:

1. **GLOSSAR.md** — neue Begriffe ans Ende anhängen (nie alphabetisch). Workflow → [[CLAUDE]]
2. **GLOSSAR-Index.md** — Term + Zeilennummer ergänzen
3. **Primär-Doku** — Submodul-Übersicht nach `references/submodul-uebersicht.md` befüllen
4. **Sekundär-Doku** — weitere Modul-Überdateien bei Cross-Module-Impact;
   [[AXIS-Session-Start]] bei neuer Modul-Navigation
5. **ADR + ADR-Index.md** — falls Architekturentscheidung ([[Rulebook_Obsidian]] §3.5)
6. **`/wikilink-check` aufrufen** — Validierungs-Endschritt

## Phase 4 — Learnings → GLOB-Domänenwissen

Jeder vom Nutzer bestätigte Delta ist ein Learning und wird in [[GLOB-Domänenwissen]]
zurückgeschrieben — so wird die nächste Hypothese besser und der nächste Entwurf hat weniger
Annahmen.

Auslöser proaktiv nach jeder Nutzer-Intervention prüfen (Korrektur, Präzisierung, Widerspruch,
ausgelassener Schritt, explizite Bestätigung eines nicht-trivialen Punkts). Falsch erkanntes
Wissen **ersetzen, nicht nur ergänzen**. Detail → `references/learnings.md`.

## Governance & Verbote

| Verbot | Quelle |
|---|---|
| Keine stillen Annahmen · Eskalationspflicht bei Geschäftsprozess-/Datenkonflikten | [[CLAUDE]] |
| Stille gilt nie als Bestätigung · nichts Unbestätigtes als Fakt propagieren | dieser Skill, Phase 2 |
| `[abgeleitet]` nur mit zitierbarer Quelle · sonst `[Annahme]` | dieser Skill, Phase 2 |
| Firmenfakt nie als gewusst behaupten — Faktenquelle ist [[GLOB-Domänenwissen]] | dieser Skill, Grundprinzip |
| `GLOSSAR.md` nie direkt laden | [[CLAUDE]] |
| Dokumente mit `status: veraltet` nie laden | [[CLAUDE]] |
| Rulebook-Konformität bei allen Vault-Updates | [[Rulebook_Obsidian]] |
