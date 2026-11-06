# Skills

> **AI agents:** If you were asked to install these skills, read [INSTALL.md](INSTALL.md) and follow it step by step.

Meine Basis-Skills für jeden KI-Coding-Agenten (Claude Code, Codex, Cursor, Gemini CLI, Copilot, OpenCode …). Das Repo enthält keine Kopien, nur die Anleitung. Der Agent holt alles direkt aus den Original-Repos, deshalb ist es immer aktuell.

## Neuer Rechner

Diesen Satz in den Agenten kopieren:

```
Installiere meine Skills: Lies https://raw.githubusercontent.com/EywaSahbi/Skills/main/INSTALL.md und befolge die Anleitung.
```

Danach den Agenten neu starten. Nutzt du auf dem Rechner mehrere Agenten, gibst du den Satz in jedem einmal ein.

## Was installiert wird

| Skill | Wofür |
|---|---|
| Superpowers | Erst planen, dann bauen: Brainstorming, Plan, Tests, Review, systematisches Debugging |
| frontend-design | Oberflächen mit eigenem Stil statt KI-Einheitslook |
| Karpathy-Guidelines | Weniger Overengineering, gezielte Änderungen, Ergebnis prüfen |
| Context7 | Aktuelle Doku zu Bibliotheken statt veraltetem Wissen |
| Deep Research | `/research`: gründliche Web-Recherche mit mehreren Agenten |
| claude-md-management | Hält CLAUDE.md-Dateien sauber (nur Claude Code) |

**Optional** (nur auf Zuruf, z. B. „installiere zusätzlich ui-ux-pro-max aus meinem Skills-Repo“): ui-ux-pro-max, context-engineering, marketing-skills, trading.

## Updates

Der Agent richtet beim Installieren einen täglichen Job ein, der alle Skills aus den Original-Repos aktualisiert. Plugins aus dem offiziellen Claude-Katalog aktualisieren sich ohnehin selbst. Das Protokoll liegt in `~/.agents/update-skills.log`.

## Liste ändern

Nur [INSTALL.md](INSTALL.md) bearbeiten: Zeile in der Tabelle ergänzen und kurz schreiben, wie installiert wird. Danach committen und pushen.

## Sicherheit

Der Agent führt aus, was in INSTALL.md steht. Gib ihm deshalb nur dieses Repo und lass niemand anderen hineinschreiben.
