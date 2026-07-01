# Architecture · #architecture

Pipeline d'**auto-critique structurée + grounding** (≠ vraie diversité cognitive).
Voir [[00-index]], [[roles]], [[decisions]].

## Flux
`Routeur → (grounding Empiriste) → Stage 1 cartes → normalisation+anonymisation
→ Stage 2 revue sur cartes → débat focalisé (complexe) → Chairman dissident → Garde-fou`

## Niveaux de routage
Le Routeur émet aussi `grounding_requis` (oui/non) qui pilote l'appel à l'Empiriste.
- **TRIVIALE** → réponse directe (+ mini-grounding si `grounding_requis: oui`, sinon drapeau « non sourcé »).
- **STANDARD** → 2 personas de débat (Adversaire, Pragmatique) + Empiriste en grounding.
- **COMPLEXE** → 4 personas de débat (Adversaire, Pragmatique, Divergent, Hypersystématique)
  + Empiriste en grounding + débat focalisé.

L'Empiriste n'est **pas** un persona de débat (il produit le dossier de faits, pas une carte).

## Modèles
2 Opus (Adversaire, Chairman) · Sonnet (orchestrateur + personas) · Haiku (routeur, revue, garde-fou).

## Distribution
**Plugin Claude Code installable** (#plugin) : `skills/conseil/` + `agents/*.md` à la racine,
manifeste `.claude-plugin/plugin.json`, catalogue `.claude-plugin/marketplace.json`.
Install via `/plugin marketplace add` + `/plugin install conseil-ia@conseil-ia`. Voir [[decisions]] (D10).

## Risque central
**Faux consensus** en mono-Claude → mitigé par grounding + Chairman dissident.
Vraie diversité de modèles = V3 multi-fournisseurs. Voir [[decisions]].
