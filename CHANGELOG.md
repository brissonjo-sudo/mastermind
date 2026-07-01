# Changelog

Toutes les évolutions notables de Conseil-IA. Format inspiré de
[Keep a Changelog](https://keepachangelog.com/), versionnage [SemVer](https://semver.org/).

## [2.1.0] — 2026-07-01

### Changed — audit du skill `conseil` (alignement spec/réalité + intégrité épistémique)
- **Flag `grounding_requis` câblé.** L'Étape 1 est désormais pilotée par le flag du Routeur.
  Une question **TRIVIALE mais factuelle** passe par une mini-vérification Empiriste, ou
  répond avec un drapeau **« non sourcé »** — plus de fait tiré de la mémoire sans signalement.
- **Personas énumérés par niveau** et correction **« 5 → 4 personas de débat »** :
  STANDARD = {adversaire, pragmatique}, COMPLEXE = {adversaire, pragmatique, divergent,
  hypersystematique}. L'Empiriste n'est **pas** un persona de débat (il fait le grounding).
- **Sélection de la divergence définie.** `revue-croisee` émet un champ `divergences`
  (paires de cartes aux thèses opposées) qui pilote l'Étape 5 (avant : métrique inexistante).
- **Mécanique du débat spécifiée.** L'Étape 5 ré-invoque les personas source via la table
  interne d'anonymisation ; **400 tokens max par camp**.
- **Anonymisation contrainte au style.** Interdiction explicite de modifier thèse/preuves/
  confiance/drapeaux (protège « l'Orchestrateur n'arbitre jamais le fond »).

### Notes
- Correspond à l'ADR **D11** (`obsidian/decisions.md`). **Zéro régression** : aligne la spec
  sur le comportement déjà observé dans `logs/2026-06-15-2310.md`.

## [2.0.0] — 2026-06-29

### Changed
- **Packaging en plugin Claude Code installable.** Le projet devient un artefact
  distribuable via marketplace locale, sans régression fonctionnelle.
  - Manifeste `.claude-plugin/plugin.json` (schéma officiel) + catalogue
    `.claude-plugin/marketplace.json`.
  - Composants déplacés à la racine du plugin : `.claude/skills/` → `skills/`,
    `.claude/agents/` → `agents/` (frontmatter strictement inchangé).
- Les 9 sous-agents conservent leur `model:` dédié (opus ×2 : adversaire, chairman ;
  sonnet ×4 : empiriste + pragmatique + divergent + hypersystematique ;
  haiku ×3 : routeur, revue-croisee, garde-fou)
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
