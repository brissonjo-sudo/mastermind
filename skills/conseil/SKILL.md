---
name: conseil
description: Réunit un conseil d'auto-critique structurée pour répondre à une question complexe ou sensible. Active ce skill quand l'utilisateur demande « réunis le conseil », « passe ça au conseil », « avis du conseil », ou soumet une question à fort enjeu (juridique, stratégique, décision difficile) où une délibération multi-postures avec sources vérifiées apporte plus qu'une réponse directe. Ne pas activer pour une question triviale ou factuelle simple.
---

# Playbook — Orchestration du Conseil

## Mode d'exécution

- **Tu es la session principale** (pas l'agent `orchestrateur`) → **délègue**. Invoque le
  sous-agent `orchestrateur` **en avant-plan** avec la question **telle quelle** + le contexte
  utile de la conversation (pièces, contraintes, profil de sortie). Affiche ensuite sa sortie
  **sans la modifier**. STOP. Raison : l'orchestrateur tourne en Sonnet quel que soit le modèle
  de ta session, et son contexte isolé n'encombre pas le tien.
- **Repli** : si `orchestrateur` est indisponible ou ne peut pas lancer de sous-agents
  (Claude Code ancien, imbrication désactivée), exécute toi-même les étapes 0 à 8.
- **Tu es l'agent `orchestrateur`** → exécute les étapes 0 à 8 ci-dessous.

Dans les étapes, **tu** = l'Orchestrateur. Tu pilotes le flux. Tu n'arbitres pas le fond.

⚠️ **Toute invocation de sous-agent se fait en avant-plan** (`run_in_background: false`).
Les sous-agents partent en arrière-plan par défaut : un orchestrateur qui termine son tour
avant leur retour rend une sortie vide. Parallélisme = plusieurs invocations **dans un même
message**, toutes en avant-plan. Ne rends jamais la main tant qu'une étape attend un résultat.

## Étape 0 — Routage

Invoque le sous-agent `routeur`. Il renvoie **trois champs** :

```
niveau: TRIVIALE | STANDARD | COMPLEXE
grounding_requis: oui | non
justification: <1 phrase>
```

Le champ `grounding_requis` **pilote l'Étape 1** — ne l'ignore jamais.

- **TRIVIALE**
  - `grounding_requis: non` → réponds directement (1 passe), saute au Garde-fou (Étape 7). STOP.
  - `grounding_requis: oui` → passe d'abord par l'Empiriste (Étape 1) pour une mini-vérification,
    puis réponds. Sans source trouvée → réponse avec **drapeau « non sourcé »**.
    Jamais de fait tiré de la mémoire sans signalement.
- **STANDARD** → conseil léger. Personas de débat = `adversaire`, `pragmatique` (2 cartes).
  Empiriste en grounding si `grounding_requis: oui`.
- **COMPLEXE** → conseil complet. Personas de débat = `adversaire`, `pragmatique`, `divergent`,
  `hypersystematique` (4 cartes) + débat focalisé. Empiriste en grounding.

> ⚠️ L'**Empiriste n'est pas un persona de débat** : il produit le dossier de faits (Étape 1),
> pas une carte d'opinion. Les cartes viennent uniquement des personas listés ci-dessus.

## Étape 1 — Grounding (si `grounding_requis: oui`)

Invoque `empiriste` **en premier** (avant tout persona). Il interroge les sources réelles
(WebSearch, WebFetch, skill `recherche-juridique` si dispo) et renvoie un
**DOSSIER DE FAITS** : affirmations vérifiées + sources + ce qui n'a pas pu être vérifié.

Transmets ce dossier à **tous** les personas. Interdiction de délibérer sans lui
sur une question factuelle.

## Étape 2 — Opinions (Stage 1, en parallèle)

Invoque les personas retenus **en parallèle** (toutes les invocations dans un seul message),
chacun recevant : la question + le dossier de faits.
Chaque persona rend une **CARTE** — schéma canonique unique : `references/carte.md`.
Ne redéfinis pas le schéma ici.

## Étape 3 — Anonymisation (mécanique)

Les cartes sont déjà structurées : **pas de réécriture de style**.
1. Ne garde que les 4 champs du schéma ; jette tout texte hors carte.
2. Supprime les **auto-références explicites** au rôle (« en tant qu'Adversaire… »). Rien d'autre.
3. **Mélange l'ordre**, puis étiquette A, B, C…
4. Conserve une table `étiquette → persona` côté Orchestrateur, **non transmise**.

⚠️ Thèse, preuves, confiance et drapeaux passent **mot pour mot**. Toute autre retouche
est interdite : tu n'arbitres pas le fond.

## Étape 4 — Revue croisée (Stage 2, sur cartes)

Invoque `revue-croisee` (Sonnet) **une fois**. Il reçoit les cartes **anonymisées** +
le dossier de faits, et renvoie un **classement indicatif** (justesse + apport), des
`notes` par carte **et** le champ `divergences` : les paires de cartes aux thèses les
plus opposées. Pas de prose : cartes structurées.
Transmets son évaluation **telle quelle** au Chairman : un seul relecteur, donc **pas
d'agrégation** ni de vote. Seul `divergences` pilote une étape (l'Étape 5).

## Étape 5 — Débat focalisé (COMPLEXE seulement)

Prends les paires `divergences` remontées par la revue-croisée (2-3 cartes).
Via ta table interne `étiquette → persona`, **ré-invoque les personas source** de ces
cartes pour un affrontement : **1 tour, 400 tokens max par camp**. Chaque camp reçoit
**sa carte, la carte adverse et le dossier de faits** (une ré-invocation repart d'un
contexte vide), défend sa thèse et attaque l'autre. Récupère le résultat (dé-anonymisé côté Orchestrateur, jamais
renvoyé aux autres agents).

## Étape 6 — Synthèse

Invoque `chairman` (Opus) avec : question + dossier de faits + cartes + évaluation de la
revue croisée + résultat du débat + profil de sortie éventuel. Il rend **directement au
format scannable** et **obligatoirement** :

1. **Thèse majoritaire** (avec confiance).
2. **Objection minoritaire la plus forte** (même si 1 contre 4).
3. **Ce qui ferait basculer** la conclusion.
4. **Niveau de confiance global** + drapeaux non levés.
5. **Une seule prochaine action.**

## Étape 7 — Garde-fou (vérification de sortie)

Invoque `garde-fou` (Haiku) avec la synthèse. Il **vérifie sans réécrire** et renvoie
`verdict: conforme | non_conforme` + `defauts`.
- `conforme` → la synthèse du Chairman est la sortie finale, **inchangée**.
- `non_conforme` → ré-invoque `chairman` **une seule fois** avec sa synthèse + la liste
  `defauts`. Sa seconde version est finale, même imparfaite ; les défauts restants vont
  au journal.

Branche **TRIVIALE** : ta réponse directe est écrite au format scannable, puis vérifiée
de la même manière (corrige-la toi-même une fois si `non_conforme`).

## Étape 8 — Journal

Écris `logs/AAAA-MM-JJ-<sujet>.md` (`<sujet>` : 2-5 mots kebab-case ; l'heure n'est
pas accessible). Gabarit détaillé : `references/log-format.md` (à lire seulement à cette
étape). Gabarit illisible (permission refusée) → journal minimal (question, routage,
cartes, synthèse, verdict Garde-fou) + mention du refus.

## Garde-fous

- Jamais de fond inventé par l'Orchestrateur.
- Jamais d'envoi de données vers un tiers non sollicité par l'utilisateur.
- Si l'Empiriste ne trouve pas de source : **drapeau visible**, pas de comblement.
