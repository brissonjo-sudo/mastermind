# Format du journal — `logs/AAAA-MM-JJ-<sujet>.md`

Référence chargée uniquement à l'Étape 8 (journalisation), pas à chaque invocation
du skill — allège le coût toujours-actif du playbook.

## Gabarit

```markdown
# Journal de conseil — AAAA-MM-JJ

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
Classement indicatif : **<A > B > ...>**
Divergences repérées : <paires, ou "aucune">
Notes : <justesse / apport par carte, 1 ligne chacune>

## Débat focalisé (si COMPLEXE)
<résumé de l'affrontement par paire ; convergence/désaccord identifié>

## Synthèse (chairman)
- **Majorité :** <thèse + confiance>
- **Dissidence préservée :** <objection minoritaire>
- **Bascule :** <ce qui changerait la conclusion>
- **Confiance globale :** <niveau> — <piège éventuel à noter>
- **Prochaine action :** <action unique>

## Garde-fou
Verdict : conforme | non_conforme (→ 1 reprise Chairman) — défauts restants : <liste ou "aucun">

## Drapeaux non levés
<liste des incertitudes non résolues>

## Avertissement
<rappel du risque de faux consensus mono-Claude si pertinent>
```

## Règles

- Une section vide (ex. pas de débat focalisé en STANDARD) : omets-la plutôt que
  d'écrire "N/A" — garde le journal scannable.
- `<sujet>` : 2 à 5 mots en kebab-case tirés de la question (l'heure n'est pas accessible aux agents). Si le fichier existe, suffixe `-2`, `-3`…
- Le journal reste **local** : `logs/` est ignoré par git (questions potentiellement sensibles).
