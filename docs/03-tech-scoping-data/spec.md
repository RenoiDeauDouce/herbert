# 03 – Cadrage technique et architecture des données d'alpha.1

**Statut** : en cours d'écriture. La section 1 est validée le 20 novembre 2026 ; les sections 2 à 7 restent à écrire.
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
