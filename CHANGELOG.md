# Changelog

Toutes les évolutions notables de Conseil-IA. Format inspiré de
[Keep a Changelog](https://keepachangelog.com/), versionnage [SemVer](https://semver.org/).

## [2.3.0] — 2026-10-03

### Added
- **Agent `orchestrateur`** (Sonnet) : la session principale lui délègue le conseil, quel
  que soit son modèle. Repli : exécution directe si l'imbrication est indisponible.
- **Banc d'essai `evals/`** : protocole A/B à l'aveugle conseil vs Opus seul + web.
- `skills/conseil/references/carte.md` : schéma de CARTE canonique, embarqué dans le plugin.

### Changed
- Frontmatter : `omitClaudeMd: true` sur tous les agents, `effort` explicite, `maxTurns`.
- **Revue croisée** : Haiku → Sonnet, reçoit le dossier de faits, classement indicatif ;
  plus d'« agrégation » des classements (un seul relecteur).
- **Anonymisation mécanique** (cartes mot pour mot, ordre mélangé) au lieu d'une
  réécriture de style.
- **Chairman** : écrit directement la sortie scannable + bloc « prochaine action ».
- **Garde-fou** : vérifie sans réécrire (verdict + défauts) ; 1 reprise Chairman max.
- `logs/*` ignoré par git ; le journal existant est retiré de l'index (conservé en local).
- Journaux nommés `logs/AAAA-MM-JJ-<sujet>.md` : l'heure n'est pas accessible aux agents.

### Fixed (trouvé au test de fumée)
- L'orchestrateur lançait ses sous-agents en arrière-plan (défaut) et rendait la main
  avant leur retour → sortie vide. Toute invocation est désormais en avant-plan.

### Vérifié
- Test de fumée headless (`claude -p --plugin-dir`, CLI 2.1.288, dossier neutre) :
  skill → `orchestrateur` → `routeur` → `garde-fou` → journal. Branche TRIVIALE seulement ;
  les branches STANDARD/COMPLEXE (Opus) n'ont pas été exécutées.

### Notes
- **Prérequis : Claude Code ≥ 2.1.271.** ADR **D13** (`obsidian/decisions.md`).
- Changement de comportement : la sortie finale est celle du Chairman, non plus une
  reformulation du Garde-fou.

## [2.2.0] — 2026-07-01

### Changed — audit du skill `conseil`, correctifs qualité/token (DRY + progressive disclosure)
- **Schéma de CARTE : source unique.** L'Étape 2 du SKILL ne redéfinit plus le schéma
  (auparavant dupliqué avec `CLAUDE.md`) ; elle référence désormais `CLAUDE.md` §
  *Schéma de CARTE*, seule source canonique. Les agents personas gardent leurs propres
  exemples de CARTE adaptés à leur rôle (contenu différent, pas une duplication à fusionner).
- **Format de log en progressive disclosure.** Le gabarit détaillé du journal est déplacé
  dans `skills/conseil/references/log-format.md` (chargé seulement à l'Étape 8), au lieu
  d'être décrit inline dans le playbook — allège le coût toujours-actif du skill.
- Parallélisme des invocations personas (Étape 2) déjà explicité en 2.1.0, confirmé sans
  changement supplémentaire.

### Notes
- Correspond à l'ADR **D12** (`obsidian/decisions.md`). Complète l'audit du skill (D11) :
  aucun changement de comportement, uniquement structure/coût de la documentation.

## [2.1.0] — 2026-07-01

### Changed — audit du skill `conseil` (alignement spec/réalité + intégrité épistémique)
- **Flag `grounding_requis` câblé.** L'Étape 1 est désormais pilotée par le flag du Routeur.
  Une question **TRIVIALE mais factuelle** passe par une mini-vérification Empiriste, ou
  répond avec un drapeau **« non sourcé »** — plus de fait tiré de la mémoire sans signalement.
- **Personas énumérés par niveau** et correction **« 5 → 4 personas de débat »** :
  STANDARD = {adversaire, pragmatique}, COMPLEXE = {adversaire, pragmatique, divergent,
  hypersystematique}. L'Empiriste n'est **pas** un persona de débat (il fait le grounding).
- **Sélection de la divergence définie.** `revue-croisee` émet un champ `divergences`
  (paires de cartes aux thèses opposées) qui pilote l'Étape 5 (avant : métrique inexistante).
- **Mécanique du débat spécifiée.** L'Étape 5 ré-invoque les personas source via la table
  interne d'anonymisation ; **400 tokens max par camp**.
- **Anonymisation contrainte au style.** Interdiction explicite de modifier thèse/preuves/
  confiance/drapeaux (protège « l'Orchestrateur n'arbitre jamais le fond »).

### Notes
- Correspond à l'ADR **D11** (`obsidian/decisions.md`). **Zéro régression** : aligne la spec
  sur le comportement déjà observé dans `logs/2026-06-15-2310.md`.

## [2.0.0] — 2026-06-29

### Changed
- **Packaging en plugin Claude Code installable.** Le projet devient un artefact
  distribuable via marketplace locale, sans régression fonctionnelle.
  - Manifeste `.claude-plugin/plugin.json` (schéma officiel) + catalogue
    `.claude-plugin/marketplace.json`.
  - Composants déplacés à la racine du plugin : `.claude/skills/` → `skills/`,
    `.claude/agents/` → `agents/` (frontmatter strictement inchangé).
- Les 9 sous-agents conservent leur `model:` dédié (opus ×2 : adversaire, chairman ;
  sonnet ×4 : empiriste + pragmatique + divergent + hypersystematique ;
  haiku ×3 : routeur, revue-croisee, garde-fou)
  et leur principe de moindre privilège (`Read, Grep, Glob` ; web réservé à l'Empiriste).

### Installation
```sh
claude plugin validate ./mastermind --strict
/plugin marketplace add ./mastermind
/plugin install conseil-ia@conseil-ia
```

### Notes
- Ce repackaging correspond à l'ADR **D10 — Packaging plugin** (`obsidian/decisions.md`).
- Lignée « V2 » du pipeline conservée (cf. README) : aucune modification de la logique
  d'orchestration, qui reste dans le skill `conseil`.
