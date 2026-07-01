# Décisions · #decision

Journal des choix d'architecture. Voir [[architecture]], [[roles]].

## D1 — Diversité par rôles, pas par marques
Simuler des marques (Grok, Gemini…) = théâtre. On différencie par **postures de raisonnement**.

## D2 — Chairman séparé de l'Orchestrateur
Intégrité épistémique : l'animateur ne juge pas son propre débat.

## D3 — Mono-Claude en V1/V2
Vraie diversité de modèles = V3 multi-fournisseurs (API tierces). Hors périmètre pour l'instant.

## D4 — Grounding obligatoire *(revue 7 IA — critique)*
Empiriste outillé (web + Légifrance) **avant** délibération. Sans lui : faux consensus.
C'est la décision la plus importante : elle distingue l'outil d'un amplificateur d'hallucinations.

## D5 — Routeur de complexité *(7/7 des IA)*
Pas 14 appels pour une question triviale. 3 voies : triviale / standard / complexe.

## D6 — Stage 2 sur cartes *(7/7)*
Revue sur cartes structurées, pas la prose → casse le coût O(n²).

## D7 — Adversaire = Sceptique + Red-team fusionnés
Recouvrement unanime (7/7). Fusion. **Hypersystématique gardé séparé** (autre plan).

## D8 — Chairman expose la dissidence
Format imposé : majorité + objection minoritaire + bascule + confiance. Contre le faux consensus et le SPOF.

## D9 — Modèles : 2 Opus
Adversaire + Chairman uniquement (5/7 des IA). Orchestrateur Sonnet (argument parsing/style, non réfuté).

## Vérifié — pas de bridage des sous-agents
Chaque agent porte son `model:` en frontmatter (doc Claude Code). Orchestrateur Sonnet
n'oblige pas les sous-agents à Sonnet. Seul `model` vide = `inherit`. Toujours expliciter Opus.

## D10 — Packaging plugin *(distribution)*
Conseil-IA devient un **plugin Claude Code installable** (un seul artefact). Layout racine
conventionnel : `skills/` + `agents/` à la racine, manifeste `.claude-plugin/plugin.json`,
catalogue `.claude-plugin/marketplace.json` (marketplace locale, `source: "./"`).
Le frontmatter des agents est **inchangé** → les `model:` dédiés sont préservés (zéro régression
sur l'archi multi-modèles, cf. [[roles]], [[architecture]]). Install : `/plugin marketplace add` +
`/plugin install conseil-ia@conseil-ia`. Versionné `2.0.0` (lignée V2). #plugin

## D11 — Alignement spec/réalité + anti-hallucination triviale *(audit skill)*
Audit du skill `conseil`. Défauts corrigés (le SKILL divergeait du comportement réel prouvé
par `logs/2026-06-15-2310.md`) :
- **`grounding_requis` câblé** ; TRIVIALE factuelle → mini-grounding ou drapeau « non sourcé ».
- **Personas énumérés** par niveau ; correction **« 5 → 4 » personas de débat** ; Empiriste ≠ persona.
- **`revue-croisee` émet `divergences`** (paires opposées) → pilote l'Étape 5 (métrique auparavant absente).
- **Débat spécifié** : ré-invocation des personas source via table interne, 400 tok/camp.
- **Anonymisation style-only** : interdiction de toucher thèse/preuves/confiance/drapeaux (intégrité épistémique).
Zéro régression, versionné `2.1.0`. Voir [[architecture]], [[roles]]. #skill #decision

## Ouvert — V3
Brancher de vrais modèles externes (multi-fournisseurs) pour décorréler le **savoir**, pas seulement le raisonnement.
