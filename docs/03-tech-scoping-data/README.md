# 03 – Cadrage technique et architecture des données

**Statut** : à écrire. Le plan ci-dessous est provisoire, à valider pendant le brainstorming.

**Rôle** : dire comment `alpha.1` est construit, quelles données il produit, et ce qui ne doit jamais en sortir.

**Dépend de** : 01 (le périmètre d'`alpha.1`). Se travaille en parallèle de 02.

**Contrat public** : les schémas vivent dans [`schemas/`](schemas/README.md).

## Décisions prises

| Date | Décision | Raison | Source |
| --- | --- | --- | --- |
| 20 novembre 2026 | La validation humaine se tient dans l'orchestrateur. `choose` prépare la carte de décision ; l'orchestrateur demande le décideur, appelle `human-gate` et écrit `decision_recorded`. | Un sous-agent de Claude Code ne peut pas interroger l'utilisateur : l'outil `AskUserQuestion` lui est retiré. | <https://code.claude.com/docs/en/sub-agents> |
| 20 novembre 2026 | Journal : trois signaux de créativité (`creative_option`, `problem_reframed`, `option_divergence`) et quatre champs fermés par option. | Mesurer la créativité sans juger le contenu des options. | [01, section 1](../01-product-management/spec.md) |

## Sections prévues

1. Composants : orchestrateur, quatre sous-agents, skills internes, hooks.
2. Contrat public : `problem`, `option`, `decision`, `attest_card`, événements du journal.
3. Journal et métriques : événements, champs, conservation (Q-06).
4. Mémoire des décisions : emplacement et règles d'écriture (Q-03, C-04).
5. Skills tiers : épinglage, adaptateur, mode dégradé, audit et licence (Q-04, Q-09).
   - Manuel des sous-agents : gabarit à six rubriques, garde-fous G-01 à G-07 en source unique, assemblage par script (critères A9 et A10 de la spec 01).
6. Confidentialité et sécurité : journal local, aucune donnée personnelle, aucun appel réseau non déclaré.
7. Environnements d'agent visés (Q-08).
