# Changelog

Toutes les évolutions notables de Conseil-IA. Format inspiré de
[Keep a Changelog](https://keepachangelog.com/), versionnage [SemVer](https://semver.org/).

## [2.0.0] — 2026-06-29

### Changed
- **Packaging en plugin Claude Code installable.** Le projet devient un artefact
  distribuable via marketplace locale, sans régression fonctionnelle.
  - Manifeste `.claude-plugin/plugin.json` (schéma officiel) + catalogue
    `.claude-plugin/marketplace.json`.
  - Composants déplacés à la racine du plugin : `.claude/skills/` → `skills/`,
    `.claude/agents/` → `agents/` (frontmatter strictement inchangé).
- Les 9 sous-agents conservent leur `model:` dédié (opus ×2 : adversaire, chairman ;
  sonnet ×5 : empiriste + 4 personas ; haiku ×3 : routeur, revue-croisee, garde-fou)
  et leur principe de moindre privilège (`Read, Grep, Glob` ; web réservé à l'Empiriste).

### Installation
```sh
claude plugin validate ./mastermind --strict
/plugin marketplace add ./mastermind
/plugin install conseil-ia@conseil-ia
```

### Notes
- Ce repackaging correspond à l'ADR **D10 — Packaging plugin** (`obsidian/decisions.md`).
- Lignée « V2 » du pipeline conservée (cf. README) : aucune modification de la logique
  d'orchestration, qui reste dans le skill `conseil`.
