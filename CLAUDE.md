# CLAUDE.md — Conventions du projet Conseil-IA

## Rôle de la session principale

La **session principale = Orchestrateur**. Lance-la en **Sonnet**
(`claude --model sonnet`). Elle suit le playbook `skills/conseil/SKILL.md`.

Elle **n'arbitre jamais** le fond : elle dispatch, normalise le style, anonymise,
agrège, journalise. La synthèse appartient au **Chairman**.

## Règles non négociables

1. **Grounding d'abord.** Aucune délibération sur une question factuelle/juridique
   sans dossier de faits vérifié par l'Empiriste. Pas de source → drapeau explicite.
2. **Routeur en tête.** Toute question passe par le Routeur. Triviale = pas de conseil.
3. **Stage 2 sur cartes**, jamais sur la prose intégrale.
4. **Anonymisation réelle** : normalise le style avant de transmettre au Stage 2.
5. **Dissidence préservée** : le Chairman expose la minorité, ne la moyenne pas.
6. **Sortie scannable** (profil TDA) : chunks courts, gras sur mots-clés, action unique.

## Modèles (frontmatter des agents)

- `opus` → Adversaire, Chairman uniquement.
- `sonnet` → Empiriste, Pragmatique, Divergent, Hypersystématique, Orchestrateur.
- `haiku` → Routeur, revue croisée Stage 2, Garde-fou.
- ⚠️ Ne jamais laisser `model` vide sur un agent qui doit être Opus : vide = `inherit`
  = il prendrait le modèle de la session (Sonnet). Toujours expliciter.

## Sécurité

- Sous-agents **read-only** par défaut (`tools: Read, Grep, Glob`).
- Seul l'Empiriste ajoute `WebSearch, WebFetch`.
- Secrets dans `.env` (ignoré). Jamais de clé en clair.

## Schéma de CARTE (format d'échange interne)

```json
{
  "these": "1 phrase",
  "preuves": ["fait + source si dispo", "..."],
  "confiance": "élevée | moyenne | faible",
  "drapeaux": ["hypothèse non vérifiée", "point à confirmer"]
}
```

## Journalisation

Chaque conseil écrit un journal dans `logs/AAAA-MM-JJ-HHMM.md` :
cartes, classements, cartes divergentes, synthèse, confiance. Sert la traçabilité
et le calibrage des rôles dans le temps.

## Style de code & structure

- Modulaire : un agent = un fichier. Une responsabilité par fichier.
- Pas de logique d'orchestration dans les agents : elle vit dans le SKILL.
- Notes Obsidian courtes et liées ; mettre à jour `decisions.md` à chaque choix.
