# Rôles · #agent

Voir [[architecture]]. Détail dans `.claude/agents/`.

| Agent | Modèle | Plan | Sortie |
|---|---|---|---|
| Routeur | haiku | tri enjeu | niveau |
| Empiriste | sonnet+web | **grounding** | dossier de faits |
| Adversaire | opus | doute + attaque | carte |
| Pragmatique | sonnet | faisabilité | carte |
| Divergent | sonnet | angle latéral | carte |
| Hypersystématique | sonnet | cohérence interne | carte |
| Revue croisée | haiku | classement Stage 2 | classement |
| Chairman | opus | synthèse + dissidence | 4 blocs |
| Garde-fou | haiku | forme accessible | sortie |
| Orchestrateur | sonnet | dispatch/anonymise | *(session)* |

## Pourquoi ces choix
- **Adversaire** = fusion Sceptique + Red-team (revue : recouvrement). Voir [[decisions]].
- **Hypersystématique gardé séparé** : plan logique distinct (cohérence ≠ vérité).
- **Orchestrateur Sonnet** : anonymisation = normalisation de style, hors Haiku.
