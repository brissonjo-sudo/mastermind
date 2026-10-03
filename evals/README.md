# Banc d'essai — le conseil bat-il un appel unique ?

**Question testée :** à question égale, le conseil complet produit-il une réponse
**plus juste et mieux calibrée** qu'un seul appel Opus avec recherche web ?
Tant que ce banc n'a pas tourné, la valeur ajoutée du conseil est une **hypothèse**.

## Conditions comparées

| | Condition A — témoin | Condition B — conseil |
|---|---|---|
| Modèle | Opus, `effort: xhigh` | pipeline Conseil-IA |
| Outils | WebSearch, WebFetch | idem (via l'Empiriste) |
| Prompt | « Réponds en citant tes sources, expose la meilleure objection, ce qui ferait basculer et ta confiance. » | « Réunis le conseil : <question> » |

Le prompt témoin demande **les mêmes rubriques** que le Chairman : on mesure la
délibération, pas le format.

## Jeu de questions (10)

À remplir dans `questions.md` (gabarit fourni). Répartition :
- **4 factuelles / juridiques à réponse connue** — réponse de référence **vérifiée par
  vous** sur source primaire (Légifrance…) **avant** de lancer le banc.
- **3 décisions à fort enjeu** sans réponse unique — jugées sur la qualité du raisonnement.
- **3 triviales** — vérifient que le Routeur n'enclenche pas le conseil (coût).

## Notation (à l'aveugle)

Mélangez les sorties A/B, masquez leur origine, notez chaque critère de 0 à 2 :

| Critère | 0 | 1 | 2 |
|---|---|---|---|
| **Justesse** (vs référence) | erreur | partiel | exact |
| **Sources** | absentes / inventées | partielles | vérifiables, en vigueur |
| **Calibrage** | sur-confiant | flou | confiance cohérente avec les preuves |
| **Dissidence** | absente | de pure forme | objection réelle et forte |
| **Actionnable** | rien | vague | une action claire |

Notez aussi **coût** (tokens, visibles dans `/usage` ou la facturation) et **durée**.

## Règle de décision

- B ≥ A + 2 points en moyenne sur les questions non triviales → le conseil se justifie.
- Écart < 2 points → réserver le conseil aux cas où la **trace écrite** (journal) a une
  valeur propre, sinon appel unique.
- Une seule **erreur factuelle** de B absente de A → priorité au grounding avant tout le reste.

## Où ranger les résultats

`evals/resultats/` (ignoré par git : les questions peuvent être sensibles).
