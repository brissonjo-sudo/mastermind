# Conseil-IA — Pipeline d'auto-critique structurée (V2)

> Inspiré de `karpathy/llm-council`, **révisé après revue critique de 7 IA**
> (Claude, ChatGPT, Gemini, Grok, Perplexity, Vibe, Manus).

---

## Ce que c'est — et ce que ce n'est PAS

**C'est :** un pipeline d'**auto-critique structurée + grounding factuel**. Plusieurs postures de raisonnement délibèrent sur une question, sur la base de **faits vérifiés**, puis une synthèse expose la position majoritaire **et** la dissidence.

**Ce n'est pas :** une vraie diversité cognitive. En **mono-Claude**, les agents partagent les mêmes connaissances → ils peuvent **halluciner ensemble**. Le système fiabilise le **raisonnement**, pas la **couverture factuelle**.

➡️ **Conséquence non négociable :** le **grounding** (sources réelles avant délibération) n'est pas une option. Sans lui, le conseil fabrique du **faux consensus**. La vraie diversité de modèles = **V3 multi-fournisseurs** (hors périmètre ici).

---

## Les 8 agents

| # | Agent | Modèle | Rôle |
|---|---|---|---|
| 1 | **Routeur** | Haiku | Classe la question : triviale / standard / complexe |
| 2 | **Orchestrateur** | Sonnet | Dispatch, **normalise le style**, anonymise, agrège *(= session principale)* |
| 3 | **Empiriste** | Sonnet + outils | **Grounding** : interroge sources réelles (web + Légifrance) |
| 4 | **Adversaire** | Opus | Doute + attaque la conclusion *(fusion Sceptique + Red-team)* |
| 5 | **Pragmatique** | Sonnet | Opérationnalité / faisabilité terrain |
| 6 | **Divergent** | Sonnet | Liens latéraux, angles créatifs |
| 7 | **Hypersystématique** | Sonnet | Traque l'incohérence interne |
| 8 | **Chairman** | Opus | Synthèse : majorité **+ dissidence + bascule** |
| — | **Garde-fou** | Haiku | Passe de sortie : format accessible / TDA |

**2 Opus** (Adversaire + Chairman). Tout le reste en Sonnet/Haiku. Orchestrateur en **Sonnet** car l'anonymisation = normalisation de style (hors de portée de Haiku).

---

## Flux

```
Question
  │
[ROUTEUR · Haiku] ── TRIVIALE ──→ 1 réponse Sonnet ──→ Garde-fou ──→ fin
  │                  STANDARD  ──→ conseil léger (3 personas)
  └──────────────────COMPLEXE ──→ conseil complet (5 personas)
  │
[EMPIRISTE] grounding : sources réelles → DOSSIER DE FAITS vérifiés
  │
STAGE 1 — Opinions (personas en parallèle, sur le dossier de faits)
  chaque persona rend une CARTE { thèse · preuves · confiance · drapeaux }
  │
[ORCHESTRATEUR · Sonnet] normalise le style + anonymise (A,B,C…)
  │
STAGE 2 — Revue croisée SUR CARTES (classement) · Haiku
  │
[ORCHESTRATEUR] agrège + isole les 2-3 cartes les plus divergentes
  │
[DÉBAT FOCALISÉ] (complexe seulement) divergents s'affrontent · 400 tokens max
  │
[CHAIRMAN · Opus] thèse majoritaire + objection minoritaire la + forte
                  + ce qui ferait basculer + niveau de confiance
  │
[GARDE-FOU · Haiku] scan accessibilité → format scannable
  │
Réponse finale  +  journal (logs/)
```

---

## Installation (plugin Claude Code)

Conseil-IA se distribue comme **plugin installable** (un seul artefact). Le manifeste vit
dans `.claude-plugin/`, les composants à la racine (`skills/`, `agents/`).

```sh
claude plugin validate ./mastermind --strict   # lint manifeste + frontmatters
/plugin marketplace add ./mastermind            # enregistre la marketplace locale
/plugin install conseil-ia@conseil-ia           # installe le plugin
```

Une fois installé : les 9 sous-agents apparaissent en `conseil-ia:<agent>` (avec leur modèle
dédié) et le skill s'active sur « réunis le conseil ». Voir `CHANGELOG.md` et
`obsidian/decisions.md` (ADR D10).

---

## Décisions clés (issues de la revue)

- **P0 — Grounding outillé** sur l'Empiriste *avant* délibération.
- **P0 — Routeur** : pas 14 appels pour une question triviale.
- **P0 — Stage 2 sur cartes** (pas la prose) : casse le coût O(n²).
- **P1 — Adversaire** : Sceptique + Red-team fusionnés (6→5 personas).
- **P1 — Chairman dissident** : expose la minorité au lieu de la moyenner.
- **P1 — Garde-fou = gabarit léger**, pas un 9ᵉ agent lourd.
- **P2 — Normalisation stylistique** avant Stage 2 (anti-réidentification).

---

## Sécurité

- **Moindre privilège** : chaque sous-agent est **read-only** (`Read, Grep, Glob`) ; seul l'Empiriste a `WebSearch, WebFetch`.
- **Secrets** : jamais dans le repo. `.env` ignoré (voir `.gitignore`). Aucune clé en clair dans les agents.
- **Données sensibles** (dossiers juridiques) : restent locales, pas de sortie vers un tiers non sollicité.

---

## Documentation

Vault Obsidian dans `obsidian/` — notes courtes, liées, taguées, optimisées pour une recherche sobre en tokens. Journal des décisions dans `obsidian/decisions.md`.
