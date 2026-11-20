# 03 – Cadrage technique et architecture des données d'alpha.1

**Statut** : en cours d'écriture. Les sections 1 et 2 sont validées le 20 novembre 2026 ; les sections 3 à 7 restent à écrire.
**Version visée** : `0.0.1-alpha.1`. **Lecteur** : le designer qui pilote `alpha.1`, seul.
**Hypothèse de départ** : rien n'est construit, ou rien qui soit connu. Si des morceaux existent, la forme dans Claude Code (section 2) s'en trouve réduite.
**Contrat public** : les schémas vivent dans [`schemas/`](schemas/README.md).

## 1. Composants et responsabilités

### Les composants

| Composant | Responsabilité | Reçoit | Rend |
| --- | --- | --- | --- |
| **Orchestrateur** `herbert` | Seul interlocuteur du designer. Pose les questions d'ouverture et de fin, enregistre le mode et l'état, appelle les sous-agents dans l'ordre, demande le décideur, appelle `human-gate`, demande la validation de toute écriture et l'exécute après accord. | La demande du designer | Les événements `session_start`, `context_change`, `decision_recorded`, `freeze_exception` |
| **`sense`** | Lit les sources, sans jamais les modifier. Chaque preuve garde son lien d'origine. | La demande, les sources | Le problème (schéma `problem`), les critères `satisfice` |
| **`devise`** | Rend chaque option dans un format commun : décision, lentille, états, cas limites, quatre champs fermés. | Le problème, les critères | Les options (schéma `option`) |
| **`choose`** | Prépare la carte de décision, sa justification et le handoff. Ne parle pas au designer. | Les options | La carte de décision (schéma `decision`, sans décideur) |
| **`review`** | Produit les cartes `attest` et le récit `narrate`, à partir des seules traces stockées. | La décision, ce qui a été livré, les données | `attest_card`, le récit, les leçons proposées |
| **`attest`, `narrate`** | Skills internes, maintenus dans le plugin. Appelés par `review`. | Les traces | La carte, le récit |
| **Skills tiers** | Un par sous-agent : `idea-refine` (`sense`), `/brainstorm` (`devise`), `human-gate` (`choose`), `skill-retro` (`review`). Appelés par un adaptateur, à une version épinglée. | La sortie du sous-agent | Une sortie traduite vers les schémas d'Herbert |
| **Hooks** | Écrivent les événements du journal. Les compteurs et les horodatages ne dépendent pas du modèle. | Les événements de session | Le journal |
| **Scripts** | Calculent ce qui ne doit pas dépendre du modèle : `option_divergence`, contrôle du décideur, assemblage des manuels, comparaison des gabarits de texte. | Les objets structurés | Les valeurs et les contrôles |
| **Mémoire des décisions** | Un fichier local par session, écrit après l'accord du designer. | La décision | Le fichier |
| **Règle `satisfice`** | Pas un composant : chaque sous-agent écrit ses critères avant d'agir et ne s'arrête que sur eux. Les critères entrent au journal avant le premier appel de skill. | Le mode, l'état | Les critères |

### Cinq règles de responsabilité

1. **Seul l'orchestrateur parle au designer.** Un sous-agent de Claude Code ne peut pas l'interroger : l'outil `AskUserQuestion` lui est retiré ([doc des sous-agents](https://code.claude.com/docs/en/sub-agents)).
2. **Seul l'orchestrateur écrit `decision_recorded`**, et seulement avec un décideur nommé (A12 du document 01, G-04).
3. **Aucun sous-agent ne modifie une source.** `sense` lit seulement. Une écriture proposée par un sous-agent figure dans son objet structuré ; l'orchestrateur la soumet au designer (G-05) puis l'exécute, et le hook la journalise.
4. **Ce qui juge est calculé par script**, pas déclaré par le modèle : la divergence des options, la lentille complète, les critères atteints (C-01, A2 du document 01). Le modèle rend un objet structuré, le hook et le script en tirent la valeur.
5. **Un sous-agent s'arrête sur ses critères `satisfice`**, et sur rien d'autre.

### Décisions de la section

| Décision | Choix | Source |
| --- | --- | --- |
| Exécution d'une écriture accordée | Par l'orchestrateur, après accord | Choix du designer, le 20 novembre 2026 |
| État du code | Hypothèse : rien n'est construit | À confirmer |

## 2. Contrat public

Les schémas `problem`, `option`, `decision`, `attest_card` et les événements du journal forment le contrat public. Ils sont versionnés en `0.0.0`. Les trois signaux de créativité et les quatre champs fermés d'une option y entrent avant le gel du 23 novembre ; ensuite, tout changement suit les exceptions X-01 à X-05. Les noms de champs sont ceux de ce document ; un champ sans source porte « À renseigner ».

### `problem` (produit par `sense`)

| Champ | Contenu | Contrôle |
| --- | --- | --- |
| `mode` | Rush ou Pace | Valeur fermée |
| `etat` | Assumption, Signal ou Evidence | Valeur fermée |
| `qui` | La personne ou le groupe concerné | Texte non vide, ou « À renseigner » |
| `blocage` | Ce qui empêche, formulé en une phrase | Texte non vide |
| `preuves` | Liste : chaque preuve porte son lien d'origine | 100 % des preuves ont une source ; sinon la liste est vide et `hypothese_a_tester` est rempli |
| `hypothese_a_tester` | Obligatoire quand aucune preuve n'existe | Présente si `preuves` est vide |
| `criteres` | Les critères `satisfice` de l'étape | Écrits avant le premier appel de skill |

### `option` (produit par `devise`)

| Champ | Contenu | Contrôle |
| --- | --- | --- |
| `decision_design` | Ce qui change à l'écran ou dans le parcours | Texte non vide |
| `effet_produit`, `effet_tech`, `effet_business` | Les trois effets de la lentille, chacun avec `source` (lien) ou « À renseigner » | Les quatre lignes renseignées (G-01) |
| `etats`, `cas_limites` | États et cas limites de l'option | Listes |
| `nature_changement` | parcours, écran, contenu, règle | Valeur fermée |
| `indicateur_vise` | L'identifiant de l'indicateur formulé par `sense` | Référence existante |
| `cout_tech` | faible, moyen, élevé | Valeur fermée |
| `levier_business` | revenu, coût, risque | Valeur fermée |
| `lentille_complete` | oui ou non | **Calculé par script**, jamais déclaré par le modèle |

### `decision` (préparé par `choose`, complété par l'orchestrateur)

| Champ | Contenu | Qui l'écrit |
| --- | --- | --- |
| `option_retenue` | La référence de l'option | `choose` (proposée), designer (tranchée) |
| `justification` | Pourquoi cette option | `choose` |
| `tension` | Les deux effets qui se contredisent, s'il y en a | `choose` |
| `handoff` | États, cas limites, critères d'acceptation | `choose` |
| `objections_bloquantes` | Liste, vide pour que `choose` s'arrête | `choose` |
| `decideur` | Le nom de la personne qui tranche | **L'orchestrateur seul** ; reste dans le fichier de décision, absent du journal et du récit (A5 du document 01) |

### `attest_card` (produit par `review`)

| Champ | Contenu | Contrôle |
| --- | --- | --- |
| `niveau` | output, outcome ou impact | Valeur fermée |
| `etat` | prouvé, sans preuve ou contribution | Valeur fermée ; l'impact ne vaut jamais « prouvé » seul (G-03) |
| `preuve` | Lien vers la preuve | Obligatoire si l'état est « prouvé » |
| `mesure_planifiee` | Date de la mesure, si aucune preuve | Date ou « aucune » |
| `objection` | oui ou non | Valeur fermée |
| `type` | À renseigner : la spec cite ce champ dans le journal sans dire ce qu'il contient | – |

### Règles communes

1. **Chaque objet est validé contre son schéma** avant d'être passé à l'étape suivante.
2. **Un objet invalide déclenche une seule relance automatique** du sous-agent (événement `relaunch`). S'il reste invalide, l'orchestrateur le signale au designer et s'arrête ; il ne corrige rien lui-même.
3. **Un champ sans source porte « À renseigner »**, jamais une valeur par défaut (G-01).
4. **Aucun schéma ne contient de donnée personnelle**, hors `decideur` dans le fichier de décision (G-07).
5. **Format : JSON Schema**, un fichier par schéma dans [`schemas/`](schemas/README.md), ce qui permet la validation par script.

### Décisions de la section

| Décision | Choix | Source |
| --- | --- | --- |
| Objet invalide | Une relance automatique, puis arrêt et information du designer | Recommandation validée le 20 novembre 2026 |
| Format des schémas | JSON Schema | Recommandation validée le 20 novembre 2026 |
| Champ `type` de `attest_card` | Reste « À renseigner » | Recommandation validée le 20 novembre 2026 |
