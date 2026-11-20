# 01 – Product management : spec d'alpha.1

**Statut** : proposé, à relire. Rédigé le 20 novembre 2026, avant le gel du 23 novembre.
**Version visée** : `0.0.1-alpha.1`. **Lecteur** : le designer qui pilote `alpha.1`, seul.

## 1. Périmètre

### Dedans

- L'orchestrateur et les quatre sous-agents, chacun avec ses critères `satisfice`.
- Les skills internes `attest` et `narrate`, et les schémas du contrat public.
- Quatre skills tiers, un par sous-agent. Chacun est audité (sécurité, licence), épinglé, et testé en mode dégradé.

| Sous-agent | Skill tiers retenu |
| --- | --- |
| `sense` | `idea-refine` (addyosmani/agent-skills) |
| `devise` | `/brainstorm` (knowledge-work-plugins) |
| `choose` | `human-gate` (claude-skills) |
| `review` | `skill-retro` (claude-skills) |

- Le journal local, les hooks et le bilan du vendredi.
- Trois signaux de créativité, ajoutés aux schémas `0.0.0` avant le gel :
  - `creative_option` : oui ou non, de la main du designer, en fin de session (une option qu'il n'aurait pas proposée seul).
  - `problem_reframed` : oui ou non, de la main du designer, en fin de session.
  - `option_divergence` : calculé par script, nombre de champs fermés qui diffèrent entre deux options (0 à 4).
- Pour que l'écart soit calculable, chaque option déclare quatre champs à valeurs fermées :

| Champ | Valeurs |
| --- | --- |
| Nature du changement | parcours, écran, contenu, règle |
| Indicateur visé | l'identifiant de l'indicateur formulé par `sense` |
| Coût tech | faible, moyen, élevé |
| Levier business | revenu, coût, risque |

  Un champ « À renseigner » est exclu de la comparaison. Le journal garde aussi le nombre de champs comparables.
- La validation humaine se tient dans l'orchestrateur, pas dans `choose` : un sous-agent de Claude Code ne peut pas interroger l'utilisateur. `choose` prépare la carte de décision ; l'orchestrateur demande le décideur, appelle `human-gate` et écrit `decision_recorded`.
- Un manuel par sous-agent, écrit sur un gabarit unique, et les garde-fous en source unique (voir A9 et A10).
- L'interface en conversation, avec blocs structurés, dans Claude Code seul.

### Dehors, au registre

C-01 à C-12. Dont : le panneau (C-10), la fenêtre flottante (C-12), les autres environnements d'agent (C-11), la mémoire par boîte de dépôt (C-04), tous les autres skills tiers, un par semaine après `0.0.1`.

## 2. Critères d'acceptation

Chaque critère est un énoncé vérifiable, avec sa vérification.

| N° | Critère | Vérification |
| --- | --- | --- |
| A1 | Chaque session complète écrit les trois signaux de créativité | Test de contrat sur le journal |
| A2 | `option_divergence` est recalculé par script, pas déclaré par le modèle | Mêmes options, même valeur sur deux exécutions |
| A3 | Les quatre skills tiers sont audités et épinglés, et un skill retiré déclenche le mode dégradé affiché | Audit consigné, puis test avec un skill retiré |
| A4 | Les signaux `creative_option` et `problem_reframed` coûtent une question chacun, en fin de session | Relecture d'une session de test |
| A5 | Un fichier de décision n'existe que si `decision_recorded` porte un décideur. Le nom du décideur est absent du journal et du récit | Test automatique de recherche du nom |
| A6 | Le journal porte sa date de fin dès l'ouverture, et le bilan du vendredi la rappelle | Relecture du journal et du bilan |
| A7 | Un relecteur est nommé pour H-09 avant le 23 novembre, sinon H-09 est reportée avec sa raison | Nom ou mention de report dans le registre des hypothèses |
| A8 | Un hook écrit un événement de test à l'ouverture d'une session dans Claude Code | Vérifié le 22 novembre |
| A9 | Chaque sous-agent a un manuel avec les six rubriques : identité, limites en trois niveaux (toujours, demander d'abord, jamais), principes de travail, schéma de sortie, exemples sans chiffre réaliste, repli | Test de présence des six rubriques par script |
| A10 | Les garde-fous G-01 à G-07 vivent dans un seul fichier ; un script assemble les quatre manuels, et le build échoue si le fichier committé diffère du résultat | Test du script d'assemblage |
| A11 | Les évaluations couvrent les six scénarios mode × état et quatre cas limites (source absente, entrée hors sujet, entrée trop longue, cas ambigu), contrôlés par code d'abord, chaque scénario rejoué plusieurs fois | Rapport de passage. Le nombre de reprises est « À renseigner » |
| A12 | Aucun événement `decision_recorded` n'est émis par un sous-agent | Test de contrat |

Les cibles des signaux de créativité sont « À renseigner » : aucun seuil n'est fixé avant le premier test (G-01).

## 3. Calendrier

| Date | Jalon | On passe si… | Décide |
| --- | --- | --- | --- |
| 20 novembre | Fin de l'assemblage | La définition de terminé d'`alpha.1` est cochée, ou les écarts sont notés | Le designer |
| 22 novembre | Contrôles avant gel | A3, A7, A8 vérifiés ; un skill non audité n'entre pas | Le designer |
| 23 novembre | Début du gel | Les cibles des signaux de créativité sont fixées avant la première session, sinon « À renseigner » | Le designer |
| 27 nov., 4, 11 et 18 déc. | Bilans du vendredi | KPI lus, rien modifié (le 4 décembre : lecture seule) | Le designer |
| 21 déc. – 3 janv. | Fêtes, gel maintenu | Les sessions éventuelles restent sur `alpha.1` | – |
| 4 – 8 janvier | Analyse | KR comparés aux cibles, H-01 à H-09 tranchées | Le designer |
| 11 janvier | Sortie de `0.0.1` | Critères de sortie atteints | Le designer |
| 18 janvier | Réintroduction | Un changement du registre par semaine | Le designer |

Si les critères ne sont pas atteints le 8 janvier : prolongation unique du 11 au 22 janvier, `0.0.1` le 25 janvier, réintroduction le 1er février.

## 4. Risques

| N° | Risque | Parade | Signal d'alerte |
| --- | --- | --- | --- |
| R1 | L'assemblage n'est pas fini le 23 novembre | Un skill non audité le 22 novembre n'entre pas : son sous-agent tourne en mode dégradé et le skill passe au registre | Une case « audité » non cochée le 22 novembre |
| R2 | Volume : 5 sessions par semaine sur 4 semaines donnent 20, soit KR-3.1 sans marge | Les fêtes comptent pour des sessions, plus la prolongation unique | Moins de 5 sessions la première semaine |
| R3 | Répartition : le designer ne choisit pas la combinaison mode × état (KR-3.2) | Planifier dès le départ les 12 sessions de couverture | Une combinaison à zéro au bilan du 4 décembre |
| R4 | Outcome prouvé en 4 semaines, livraison comprise (KR-2.1) | `review` accepte une mesure planifiée avec date ; la cible reste une proposition | Aucune carte `Evidence` le 4 décembre |
| R5 | Signaux 1 et 2 déclarés par le designer, donc exposés à la lassitude | Une question chacun, en fin de session, croisée avec l'écart calculé | Des réponses vides sur des sessions complètes |
| R6 | Écart non calculable : tous les champs d'une paire sont « À renseigner » | Le journal garde le nombre de champs comparables | Zéro champ comparable sur une paire |
| R7 | Un changement de règle casse la comparabilité | Gel et exceptions X-01 à X-05, chaque exception journalisée | Un événement `freeze_exception` |

## 5. Décisions du 20 novembre 2026

| Décision | Choix | Source |
| --- | --- | --- |
| Q-03 – Mémoire des décisions | Un fichier local par session, écrit après l'accord du designer (G-05). Le nom du décideur y reste. | Recommandation validée |
| Q-06 – Conservation du journal | Jusqu'à la décision de sortie (11 janvier, ou 25 janvier en cas de prolongation), puis suppression | Recommandation validée |
| Q-07 – Participants | Le designer seul, environ 5 sessions par semaine. H-09 demande un relecteur nommé | Recommandation validée |
| Q-08 – Environnement | Claude Code seul | Recommandation validée |
| Q-09 – Skills tiers | Un par sous-agent ; un skill non audité le 22 novembre n'entre pas | Choix du designer |
| Validation humaine | Dans l'orchestrateur, pas dans `choose` | [Doc des sous-agents](https://code.claude.com/docs/en/sub-agents) |
| Prompts | Dans `alpha.1` : gabarit, garde-fous en source unique, quatre cas limites | Choix du designer |
| Fenêtre flottante | Au registre (C-12), avec sa propre spec après `0.0.1` | Choix du designer |

## 6. Questions encore ouvertes

| N° | Question | Statut |
| --- | --- | --- |
| Q-01 | 2 ou 3 options par mode et état ? | Reste ouverte ; mesurée pendant le gel, tranchée à l'analyse du 4 au 8 janvier |
| Q-02 | Un axe de réversibilité ? | Reportée après `0.0.1` |
| Q-04 | Les skills font-ils ce que leur README annonce ? | Traitée pour les quatre skills d'`alpha.1` par l'audit (A3) ; les autres restent non vérifiés |
| Q-05 | Dans quel ouvrage Simon formule-t-il les quatre phases ? | Hors `alpha.1`, à vérifier avant toute citation publique |
