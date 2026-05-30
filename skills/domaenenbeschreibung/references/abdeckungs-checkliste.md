# Abdeckungs-Checkliste

Vollständigkeits-Werkzeug für den Gapcheck in Phase 2. Zwei allgemeine Ebenen, beide
**rein interrogativ** — sie erzeugen `[Lücke]`- und `[Annahme]`-Flags zum Klären, niemals
`[abgeleitet]`-Fakten. Nicht adressierte Punkte → `[Lücke]`.

Diese Liste vereint Modellierungs-Dimensionen, Standard-Abgleich und Architektur-/Machbarkeits-
Prüfung in einem Werkzeug. K1–K8 ist hier integriert — kein separater Sprint-K-Check mehr.

---

## Ebene 1 — Strukturelles Rückgrat (jedes Topic)

Universelle Modellierungs-Dimensionen — gilt unabhängig von der Domäne.

- **Identität & Abgrenzung** (K2) — Was ist die Entität? Neues Objekt vs. Änderung am alten?
  Klar abgegrenzt von verwandten Domänen-Objekten?
- **Zustände & Übergänge** (K1/K8) — Alle Status benannt? Übergänge als `Zustand → Zustand`?
  Auslöser/Bedingungen? Blockierend oder nicht? Rückwärtspfade explizit? Oder: keine
  Status-Entität.
- **Hoheit & Ownership** (K3) — Welcher Bounded Context besitzt sie? Wer darf Zustände ändern?
  Shared Entities mit Owner-BC in [[GLOB-Modul-Uebersicht]] registriert?
- **Kardinalität & Relationen** — Ebene (Position/Projekt/…)? 1:n, n:m? Beziehungen?
- **Regeln & Invarianten** (K7) — Nicht-triviale Regeln als `ID · Wenn · Dann`? IDs vergeben?
- **Ausnahmen** (K6) — Mindestens ein echter Ausnahmepfad (oder explizit keiner)? Echte
  Prozesszweige — kein Fehlerhandling, keine UI-Varianten.
- **Event vs. Gate** — Hinweis-Event oder blockierende Freigabe?
- **Systemgrenze** — Was muss ins System, was bleibt außerhalb? In-module vs. Cross-BC?

> K4/K5 nachtragen, sobald im K-Katalog definiert.

---

## Ebene 2 — Standard-ERP-Abgleich (externes Wissen der KI)

Zweck: ein zur Firma orthogonaler Blickwinkel. Die KI vergleicht den Entwurf gegen den
*typischen Soll-Umfang* eines ERP-Moduls dieser Art und deckt auf, was die Firma — und damit
ihre eingespielten Routinen — aus Gewohnheit übersehen könnte.

**Arbeitsweise:**
- Frage: „Ein Standard-ERP-Modul dieser Art behandelt üblicherweise auch X. Bei euch relevant
  oder bewusst nicht?" — als `[Lücke]`/`[Annahme]`, nie als Behauptung.
- **Relevanzfilter:** gegen Kontext prüfen — Mittelstand, die drei Geschäftsfelder
  (Werkzeugbau · Lehrenbau · Serie), der in [[GLOB-Domänenwissen]] etablierte Scope.
  Offensichtlich irrelevante Standardfunktionen (z.B. Mehrwährung, EDI, Konsignation) nicht
  flaggen, außer es gibt einen konkreten Anhaltspunkt.
- **Nicht re-litigieren:** Was in [[GLOB-Domänenwissen]] bereits bewusst entschieden ist
  (z.B. „keine Artikel-Versionierung"), nicht erneut aufrollen.
- **Priorisieren:** die wenigen wahrscheinlich-relevanten Lücken zuerst, kein Lehrbuch-Dump.
  Die Flag-Liste bleibt kurz.

**Harte Regel:** Diese Ebene darf nie `[abgeleitet]` erzeugen. „Abgeleitet woraus?" → immer
aus [[GLOB-Domänenwissen]], nie aus dem ERP-Standardwissen.

---

## Ebene 3 — Architektur & Machbarkeit

Zweck: Den fachlich beschriebenen Prozess auf architektonische Sauberkeit und technische
Machbarkeit *prüfen* — nicht implementieren. Diese Ebene **flaggt** (`[Lücke]`/`[Annahme]`/
Eskalation), sie entwirft keine Lösung. Tiefere technische Gestaltung → S2 / ADR.

### Architektur (Struktur & Schnitt)
- **Bounded-Context-Schnitt:** Liegt das Konzept im richtigen Kontext, oder verletzt es eine
  Grenze? Greift es auf fremde Repositories zu (verboten — nur über Interface)?
- **Shared Kernel:** Berührt eine Entität mehrere Bounded Contexts? → Eskalation, Owner-BC in
  [[GLOB-Modul-Uebersicht]] klären (deckt sich mit K3).
- **Sub-Modul-Granularität:** 3–8 Tabellen als Richtwert — zu grob (zu viele
  Verantwortlichkeiten) oder zu fein (Overhead ohne Mehrwert)?
- **Vertical Slice:** Ist das Feature end-to-end vollständig (Schema → UI), nicht schichtweise?
- **Abhängigkeitsreihenfolge:** Passt die Implementierungsreihenfolge zu den Modul-Abhängigkeiten?
- **Pegging:** Entstehen neue `DocumentLink`-Typen? → [[GLOB-Pegging-CodingSpec]].
- **Methodik:** kein taktisches DDD, keine Clean Architecture → [[GLOB-Einfuehrungsmethodik]].

### Machbarkeit (Stack)
- **Absolutverbote impliziert?** ([[GLOB-Tech-Stack]]) — verstecktes `DELETE` (statt
  `deletedAt`), `MAX(id)+1` (statt `SEQUENCE`), Update ohne `version`-Check (Optimistic
  Locking), Prisma im Controller, neues Package ohne ADR.
- **Transaktionsgrenzen:** Multi-Table-Write → `prisma.$transaction`? Mehr als 5 Tabellen →
  Eskalation (Deadlock/Timeout).
- **Async-Entscheidung:** Bulk-Operationen (MRP-Lauf, PDF-Generierung, FA-Split) → pg-boss;
  was kann synchron bleiben?
- **Performance-Risiken:** Recursive CTE über >4 Ebenen / >200 Artikel ohne Index, N+1
  (DB-Abfrage in Schleife), synchrone Bulk-Operationen.
- **Modul-Struktur:** Controller → Service → Repository eingehalten?
- **Aufwandsindikator** je Konzept: schnell / mittel / aufwändig / nicht ohne ADR.

---

## Pflege

- Ebene-1/2-Stichworte sind *Abdeckungswissen* (was abfragen?), kein Faktenwissen — Fakten
  leben in [[GLOB-Domänenwissen]].
- Ebene 3 flaggt nur und entwirft nicht — bei echtem Klärungsbedarf an S2/ADR übergeben.
- Stichworte knapp halten; neue ergänzen, wenn ein wiederkehrend vergessener Aspekt auffällt.
