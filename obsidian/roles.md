# Rôles · #agent

Voir [[architecture]]. Détail dans `agents/`.

| Agent | Modèle · effort | Plan | Sortie |
|---|---|---|---|
| Orchestrateur | sonnet · medium | dispatch / anonymise / journal | sortie finale + chemin du journal |
| Routeur | haiku | tri enjeu | niveau + grounding_requis |
| Empiriste | sonnet · high + web | **grounding** | dossier de faits |
| Adversaire | opus · xhigh | doute + attaque | carte |
| Pragmatique | sonnet · medium | faisabilité | carte |
| Divergent | sonnet · medium | angle latéral | carte |
| Hypersystématique | sonnet · medium | cohérence interne | carte |
| Revue croisée | sonnet · medium | évaluation Stage 2 | classement indicatif + divergences |
| Chairman | opus · xhigh | synthèse + dissidence | 5 blocs scannables |
| Garde-fou | haiku | vérif. forme accessible | verdict + défauts |

## Pourquoi ces choix
- **Adversaire** = fusion Sceptique + Red-team (revue : recouvrement). Voir [[decisions]].
- **Hypersystématique gardé séparé** : plan logique distinct (cohérence ≠ vérité).
- **Orchestrateur agent Sonnet** : indépendant du modèle de la session principale (D13).
- **Revue croisée Sonnet** : un relecteur Haiku jugeait des cartes Opus sans pouvoir vérifier les faits (D13).
- **Garde-fou vérificateur** : une réécriture Haiku du livrable final risquait de perdre des nuances (D13).
