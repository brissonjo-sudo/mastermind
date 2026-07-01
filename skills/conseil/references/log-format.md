# Format du journal — `logs/AAAA-MM-JJ-HHMM.md`

Référence chargée uniquement à l'Étape 8 (journalisation), pas à chaque invocation
du skill — allège le coût toujours-actif du playbook.

## Gabarit

```markdown
# Journal de conseil — AAAA-MM-JJ HH:MM

**Question :** « <question posée> »

## Routage
- **Niveau :** TRIVIALE | STANDARD | COMPLEXE
- **Grounding requis :** oui | non
- **Justification :** <1 phrase du routeur>

## Dossier de faits (empiriste)
<affirmations vérifiées + sources ; ou "non requis" si grounding_requis: non>

## Cartes (Stage 1) — table interne (non transmise au Stage 2)
- **A = <persona>** : <thèse résumée>. Confiance <niveau>.
- ...

## Revue croisée (Stage 2, sur cartes anonymisées)
Classement : **<A > B > ...>**
Divergences repérées : <paires, ou "aucune">

## Débat focalisé (si COMPLEXE)
<résumé de l'affrontement par paire ; convergence/désaccord identifié>

## Synthèse (chairman)
- **Majorité :** <thèse + confiance>
- **Dissidence préservée :** <objection minoritaire>
- **Bascule :** <ce qui changerait la conclusion>
- **Confiance globale :** <niveau> — <piège éventuel à noter>

## Drapeaux non levés
<liste des incertitudes non résolues>

## Avertissement
<rappel du risque de faux consensus mono-Claude si pertinent>
```

## Règles

- Une section vide (ex. pas de débat focalisé en STANDARD) : omets-la plutôt que
  d'écrire "N/A" — garde le journal scannable.
- Le nom de fichier utilise l'heure de fin de délibération, pas de début.
- Voir `logs/2026-06-15-2310.md` pour un exemple complet rempli.
