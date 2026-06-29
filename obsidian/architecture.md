# Architecture · #architecture

Pipeline d'**auto-critique structurée + grounding** (≠ vraie diversité cognitive).
Voir [[00-index]], [[roles]], [[decisions]].

## Flux
`Routeur → (grounding Empiriste) → Stage 1 cartes → normalisation+anonymisation
→ Stage 2 revue sur cartes → débat focalisé (complexe) → Chairman dissident → Garde-fou`

## Niveaux de routage
- **TRIVIALE** → 1 réponse directe, pas de conseil.
- **STANDARD** → 3 personas (Adversaire, Empiriste, Pragmatique).
- **COMPLEXE** → 5 personas + débat focalisé.

## Modèles
2 Opus (Adversaire, Chairman) · Sonnet (orchestrateur + personas) · Haiku (routeur, revue, garde-fou).

## Distribution
**Plugin Claude Code installable** (#plugin) : `skills/conseil/` + `agents/*.md` à la racine,
manifeste `.claude-plugin/plugin.json`, catalogue `.claude-plugin/marketplace.json`.
Install via `/plugin marketplace add` + `/plugin install conseil-ia@conseil-ia`. Voir [[decisions]] (D10).

## Risque central
**Faux consensus** en mono-Claude → mitigé par grounding + Chairman dissident.
Vraie diversité de modèles = V3 multi-fournisseurs. Voir [[decisions]].
