# Conseil-IA — Pipeline d'auto-critique structurée (V2)

> Inspiré de `karpathy/llm-council`, **révisé après revue critique de 7 IA**
> (Claude, ChatGPT, Gemini, Grok, Perplexity, Vibe, Manus).

---

## Ce que c'est — et ce que ce n'est PAS

**C'est :** un pipeline d'**auto-critique structurée + grounding factuel**. Plusieurs postures de raisonnement délibèrent sur une question, sur la base de **faits vérifiés**, puis une synthèse expose la position majoritaire **et** la dissidence.

**Ce n'est pas :** une vraie diversité cognitive. En **mono-Claude**, les agents partagent les mêmes connaissances → ils peuvent **halluciner ensemble**. Le système fiabilise le **raisonnement**, pas la **couverture factuelle**.

➡️ **Conséquence non négociable :** le **grounding** (sources réelles avant délibération) n'est pas une option. Sans lui, le conseil fabrique du **faux consensus**. La vraie diversité de modèles = **V3 multi-fournisseurs** (hors périmètre ici).

➡️ **Valeur ajoutée à prouver :** tant que le banc d'essai (`evals/`) n'a pas tourné, l'avantage du conseil sur un appel Opus unique avec recherche web reste une **hypothèse**.

---

## Les 10 agents

| # | Agent | Modèle · effort | Rôle |
|---|---|---|---|
| 1 | **Orchestrateur** | Sonnet · medium | Dispatch, anonymise (mécanique), journalise. Reçoit le conseil de la session principale |
| 2 | **Routeur** | Haiku | Classe la question : triviale / standard / complexe + `grounding_requis` |
| 3 | **Empiriste** | Sonnet · high + web | **Grounding** : interroge sources réelles (web + Légifrance) |
| 4 | **Adversaire** | Opus · xhigh | Doute + attaque la conclusion *(fusion Sceptique + Red-team)* |
| 5 | **Pragmatique** | Sonnet · medium | Opérationnalité / faisabilité terrain |
| 6 | **Divergent** | Sonnet · medium | Liens latéraux, angles créatifs |
| 7 | **Hypersystématique** | Sonnet · medium | Traque l'incohérence interne |
| 8 | **Revue croisée** | Sonnet · medium | Évalue les cartes anonymisées + repère les paires divergentes |
| 9 | **Chairman** | Opus · xhigh | Synthèse scannable : majorité **+ dissidence + bascule + action** |
| 10 | **Garde-fou** | Haiku | **Vérifie** complétude et format accessible / TDA, sans réécrire |

**2 Opus** (Adversaire + Chairman). Tous les agents portent `omitClaudeMd: true` : leur contexte vient du prompt de délégation, pas des `CLAUDE.md` de l'utilisateur.

---

## Flux

```
Question → session principale ──délègue──→ [ORCHESTRATEUR · Sonnet]
  │
[ROUTEUR · Haiku] ── TRIVIALE ──→ réponse directe ──→ Garde-fou ──→ fin
  │  (+ grounding_requis)  STANDARD  ──→ conseil léger (2 personas de débat)
  └──────────────────────  COMPLEXE ──→ conseil complet (4 personas de débat)
  │
[EMPIRISTE] grounding : sources réelles → DOSSIER DE FAITS vérifiés
  │
STAGE 1 — Opinions (personas en parallèle, sur le dossier de faits)
  chaque persona rend une CARTE { thèse · preuves · confiance · drapeaux }
  │
[ORCHESTRATEUR] anonymise mécaniquement (mot pour mot, ordre mélangé, A,B,C…)
  │
STAGE 2 — Revue croisée SUR CARTES (évaluation indicative + paires divergentes) · Sonnet
  │
[DÉBAT FOCALISÉ] (complexe) ré-invoque les personas source · 400 tokens max/camp
  │
[CHAIRMAN · Opus] synthèse scannable : majorité + dissidence + bascule
                  + confiance + une action
  │
[GARDE-FOU · Haiku] vérifie (conforme / non conforme → 1 reprise Chairman)
  │
Réponse finale  +  journal local (logs/)
```

---

## Installation (plugin Claude Code)

Prérequis : **Claude Code ≥ 2.1.271** (`omitClaudeMd`, sous-agents imbriqués).
Vérifier avec `claude --version`. Sous Windows, une ancienne installation npm
(`%APPDATA%\npm\claude`) peut masquer l'installation native à jour dans le PATH.

```sh
claude plugin validate ./mastermind --strict   # lint manifeste + frontmatters
/plugin marketplace add ./mastermind            # enregistre la marketplace locale
/plugin install conseil-ia@conseil-ia           # installe le plugin
```

Une fois installé : les 10 sous-agents apparaissent en `conseil-ia:<agent>` (avec leur modèle
dédié) et le skill s'active sur « réunis le conseil ». La session principale peut tourner
sur n'importe quel modèle : elle délègue à l'Orchestrateur (Sonnet). Voir `CHANGELOG.md` et
`obsidian/decisions.md` (ADR D10, D13).

---

## Décisions clés

- **P0 — Grounding outillé** sur l'Empiriste *avant* délibération.
- **P0 — Routeur** : pas 14 appels pour une question triviale.
- **P0 — Stage 2 sur cartes** (pas la prose) : casse le coût O(n²).
- **P1 — Adversaire** : Sceptique + Red-team fusionnés (6→5 personas).
- **P1 — Chairman dissident** : expose la minorité au lieu de la moyenner.
- **D13 — Orchestrateur délégué**, anonymisation mécanique, Chairman scannable +
  Garde-fou vérificateur, revue croisée en Sonnet, `effort` explicite, journaux locaux.

---

## Sécurité

- **Moindre privilège** : sous-agents **read-only** (`Read, Grep, Glob`) ; seul l'Empiriste a `WebSearch, WebFetch` ; l'Orchestrateur a `Agent, Skill, Write` (écriture limitée au journal).
- **Secrets** : jamais dans le repo. `.env` ignoré (voir `.gitignore`). Aucune clé en clair dans les agents.
- **Données sensibles** (dossiers juridiques) : restent locales. `logs/` et `evals/resultats/` sont **ignorés par git** (dépôt public).

---

## Documentation

Vault Obsidian dans `obsidian/` — notes courtes, liées, taguées, optimisées pour une recherche sobre en tokens. Journal des décisions dans `obsidian/decisions.md`. Banc d'essai dans `evals/`.
