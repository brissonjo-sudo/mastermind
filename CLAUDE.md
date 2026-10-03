# CLAUDE.md — Conventions du projet Conseil-IA

## Rôle de la session principale

La session principale, quel que soit son modèle, **délègue** le conseil à l'agent
`orchestrateur` (Sonnet), qui suit le playbook `skills/conseil/SKILL.md`.
Repli si l'imbrication de sous-agents est indisponible : la session exécute elle-même
le playbook (la lancer alors en Sonnet : `claude --model sonnet`).

L'Orchestrateur **n'arbitre jamais** le fond : il dispatch, anonymise, journalise.
La synthèse appartient au **Chairman**.

Prérequis : **Claude Code ≥ 2.1.271** (`omitClaudeMd` ; imbrication de sous-agents).

## Règles non négociables

1. **Grounding d'abord.** Aucune délibération sur une question factuelle/juridique
   sans dossier de faits vérifié par l'Empiriste. Pas de source → drapeau explicite.
2. **Routeur en tête.** Toute question passe par le Routeur. Triviale = pas de conseil.
3. **Stage 2 sur cartes**, jamais sur la prose intégrale.
4. **Anonymisation mécanique** : cartes transmises mot pour mot, auto-références au rôle
   retirées, ordre mélangé. Aucune réécriture de style.
5. **Dissidence préservée** : le Chairman expose la minorité, ne la moyenne pas.
6. **Sortie scannable** (profil TDA) : écrite par le Chairman, **vérifiée** (pas réécrite)
   par le Garde-fou.

## Modèles (frontmatter des agents)

- `opus` (`effort: xhigh`) → Adversaire, Chairman uniquement.
- `sonnet` → Orchestrateur, Empiriste (`high`), Pragmatique, Divergent,
  Hypersystématique, Revue croisée (`medium`).
- `haiku` → Routeur, Garde-fou (pas d'`effort` fixé).
- ⚠️ Ne jamais laisser `model` vide sur un agent qui doit être Opus : vide = `inherit`
  = il prendrait le modèle de la session. Toujours expliciter.
- `omitClaudeMd: true` sur tous les agents : leur contexte vient du prompt de délégation,
  pas des `CLAUDE.md` (dont les consignes de format perturberaient les cartes).

## Sécurité

- Sous-agents **read-only** par défaut (`tools: Read, Grep, Glob`).
- Seul l'Empiriste ajoute `WebSearch, WebFetch`.
- L'Orchestrateur ajoute `Agent, Skill, Write` (Write limité au journal par son prompt).
- Secrets dans `.env` (ignoré). Jamais de clé en clair.
- Plugin : les champs `hooks`, `mcpServers`, `permissionMode` des agents sont ignorés.

## Schéma de CARTE (format d'échange interne)

Source canonique : `skills/conseil/references/carte.md` (voyage avec le plugin ;
ce `CLAUDE.md` n'est pas chargé chez l'utilisateur du plugin).

## Journalisation

Chaque conseil écrit un journal dans `logs/AAAA-MM-JJ-HHMM.md` : cartes, évaluation
de la revue croisée, divergences, synthèse, verdict du Garde-fou. Sert la traçabilité et
le calibrage des rôles. **Local uniquement** : `logs/` est ignoré par git (dépôt public).

## Style de code & structure

- Modulaire : un agent = un fichier. Une responsabilité par fichier.
- Pas de logique d'orchestration dans les agents : elle vit dans le SKILL.
- Notes Obsidian courtes et liées ; mettre à jour `decisions.md` à chaque choix.
