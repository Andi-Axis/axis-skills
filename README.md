# Axis-skills

Plugin-Marketplace für das AXIS-Projekt — Claude-Code-Skills zur Vault- und Dokumentationspflege.

## Enthaltene Skills

| Skill | Aufruf | Zweck |
|---|---|---|
| Domänenbeschreibung | `/domaenenbeschreibung` | S1-Phase im AXIS-Skill-Tree: Domänenanforderungen mit 8-köpfigem KI-Key-User-Team strukturiert ermitteln |
| Wikilink-Check | `/wikilink-check` | Vault-Dokumente vor Freigabe auf Wikilink-Vollständigkeit, GLOSSAR-Abdeckung und Indexkonsistenz prüfen |

## Installation

### Lokal (Entwicklung)

```
# Marketplace registrieren (Directory-Quelle)
/plugin marketplace add C:\AXIS\Axis-skills

# Plugin installieren
/plugin install axis-skills@axis-skills
```

### Cowork / andere Maschinen (via GitHub)

```
/plugin marketplace add Andi-Axis/Axis-skills
/plugin install axis-skills@axis-skills
```

## Repository-Struktur

```
Axis-skills/
├── .claude-plugin/
│   ├── plugin.json          Plugin-Metadaten
│   └── marketplace.json     Single-Plugin-Marketplace
└── skills/
    ├── domaenenbeschreibung/
    │   └── SKILL.md
    └── wikilink-check/
        ├── SKILL.md
        └── references/
```

## Lizenz

Internes AXIS-Projekt — privates Repo.
