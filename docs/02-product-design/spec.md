# 02 – Product design : parcours et règles d'interface d'alpha.1

**Statut** : écrit, à relire. Les sections 1 à 4 sont validées le 20 novembre 2026 ; la section 5 est un brouillon écrit le même jour.
**Version visée** : `0.0.1-alpha.1`. **Lecteur** : le designer qui pilote `alpha.1`, seul.
**Source visuelle** : la [maquette Figma](https://www.figma.com/design/k498URAudOXclhXJADIkiH/Herb.ert). Elle montre la cible. Dans `alpha.1`, tout se passe en conversation, dans Claude Code seul : chaque composant est rendu en bloc de texte structuré. Le panneau (C-10) et la fenêtre flottante (C-12) sont au registre.

## 1. Parcours et états d'une session

### Le fil d'une session

| N° | Moment | Ce que voit le designer | Ce qu'il fait |
| --- | --- | --- | --- |
| 1 | Ouverture | Deux questions courtes : le temps dont il dispose, ce qu'il a en main | Répond. Herbert en déduit le mode (Rush ou Pace) et l'état (Assumption, Signal ou Evidence). |
| 2 | Contrat de séance | Les critères « suffisant » écrits par Herbert, le mode et l'état visibles | Corrige un critère, ou change le mode ou l'état, à tout moment. Herbert dit ce qu'il réécrit. |
| 3 | `sense` | Une ligne de statut, puis le problème formulé | Rien : cette étape n'attend personne. |
| 4 | `devise` | Une ligne de statut, puis un bloc par option, avec ses quatre champs fermés et les quatre lignes de la lentille | Peut demander une option de plus (`option_request`). |
| 5 | Champs manquants | Les champs « À renseigner », listés | Les remplit ou les laisse. |
| 6 | `choose` | La carte de décision, avec la tension entre deux effets | Nomme le décideur (obligatoire), puis valide. C'est le seul moment où Herbert attend une personne. |
| 7 | Écriture | Une demande de validation avant toute modification | Accepte ou refuse (G-05). |
| 8 | `review` | La carte `attest` (output, outcome, impact, les niveaux sans preuve marqués comme tels) et le récit, une trace par phrase | Lit, conteste (objection). |
| 9 | Fin de session | Deux questions : une option qu'il n'aurait pas proposée seul ? un problème reformulé ? | Répond oui ou non à chacune (`creative_option`, `problem_reframed`). |

Les champs manquants viennent avant `choose` : un « À renseigner » doit être connu avant de décider. Les deux questions de fin viennent après `review` : le designer vient de lire l'attestation et le récit, il juge mieux la session.

### Ce qui sort du fil

- **Changement de contexte** : le mode ou l'état change en cours de session (`context_change`). Herbert le dit, réécrit les critères et reprend l'étape en cours.
- **Relance** : un sous-agent qui s'arrête sans avoir atteint ses critères peut être relancé (`relaunch`). Herbert dit pourquoi il s'était arrêté.
- **Mode dégradé** : un skill tiers échoue ou manque. Un message dit lequel, et l'étape continue sans lui, ou s'arrête si aucune alternative n'existe.
- **Session incomplète** : le designer quitte avant `review`. Les deux questions de fin ne sont pas posées et `session_end` porte « session complète : non ».

## 2. Règles par composant

| Composant | États | Règles | Rendu en `alpha.1` |
| --- | --- | --- | --- |
| **Chips mode et état** | Mode : Rush, Pace. État : Assumption, Signal, Evidence | Toujours visibles et modifiables. Un changement fait réécrire les critères, et Herbert le dit. | Une ligne d'état en tête de chaque étape |
| **Rail des quatre étapes** | En attente, en cours, terminé, à reprendre | Seule l'étape `choose` peut attendre une personne ; elle se signale alors en ambre. « À reprendre » signifie que l'étape s'est arrêtée sans atteindre ses critères et peut être relancée. | Une ligne de statut par étape |
| **Liste de critères** | Verdict « en cours » ou « Suffisant » ; chaque critère coché ou non | Les critères sont écrits avant chaque étape et se cochent en direct. Quand tous le sont, Herbert dit « Suffisant » et s'arrête. Le designer peut en corriger un : Herbert réécrit la liste. | La liste avant l'étape, cochée à la fin |
| **Carte d'option** | Proposée, retenue, écartée | Quatre lignes de lentille : décision design, effet produit, effet tech, effet business. Chacune porte sa source ou « À renseigner ». Les options se posent côte à côte ; une seule est retenue, par une personne nommée. | Un bloc par option, avec ses quatre champs fermés |
| **Carte de décision** | En attente du décideur, tranchée | Tant que « Décideur » est vide, le bouton reste inactif et rien n'est enregistré (G-04). Si deux effets se contredisent, la tension est portée devant la personne. | Une question de l'orchestrateur : le décideur, puis la validation |
| **Échelle `attest`** | Par niveau : prouvé, sans preuve, contribution | Output, outcome, impact. Un niveau sans preuve reste grisé (G-02). L'impact n'est jamais attribué au design seul : il porte « contribution » (G-03). | Trois lignes, les niveaux sans preuve marqués comme tels |
| **Récit** | Phrase : reliée à sa trace, ou sans trace | Il assemble des traces déjà écrites. Chaque phrase renvoie à sa trace ; une phrase sans trace est signalée. Herbert n'écrit rien de mémoire. Six sections, tirées chacune d'une source (tableau ci-dessous). | Une référence de trace à la fin de chaque phrase (la trace au survol est au registre, C-10) |
| **Bandeau de mode dégradé** | Dégradé, bloquant | Un skill absent ou en échec est dit, jamais en silence. Dégradé : Herbert continue sans le skill. Bloquant : aucune alternative, Herbert s'arrête. | Un message en tête de l'étape concernée, qui nomme le skill |
| **Validation d'écriture** | En attente, accordée, refusée | Aucune modification de fichier, de maquette ou de skill sans accord explicite, et seulement ce qui est nécessaire (G-05). La modification se montre façon diff ; chaque écriture est journalisée et annulable. | Une demande avec le diff en texte, avant toute écriture |

### Les six sections du récit

Les titres sont ceux de la spec en ligne, gardés en anglais. Leur traduction, si elle a lieu, se décide en section 3.

| Section | Tirée de |
| --- | --- |
| Overview | Le problème formulé par `sense` |
| Constraint | Le mode, l'état et les critères `satisfice` |
| Blueprint | La décision tranchée par la personne nommée, préparée par `choose` et sa lentille |
| Reality check | Les écarts entre le prévu et le livré, les objections levées |
| Outcome | Les cartes `attest` |
| What came next | Les leçons et les mesures planifiées |

### Défaut connu dans la maquette

Quelques variantes ont des noms cassés : `État=État6`, `État=État8` et `État=Intelligence Sense` (rail, composant Étape), `Étape active=Étape active5` (rail). À corriger dans Figma avant de s'en servir pour un handoff ; sans effet sur `alpha.1`.

## 3. Contenus

Tous les textes sont ceux d'`alpha.1`, en conversation. Les `{…}` sont des valeurs que l'orchestrateur remplit à l'exécution. Aucun chiffre, seuil ou définition n'est ajouté à ce que la spec donne.

### Règles de ton

1. **Vouvoiement.** Les cartes sont lues par la personne qui décide, qui n'est pas toujours le designer.
2. **Une phrase, un fait.** Pas de formule d'accueil, pas de conclusion creuse.
3. **Herbert dit ce qu'il ne sait pas.** « Je ne sais pas » ou « À renseigner », jamais un trou comblé (G-01).
4. **Un message dit quoi, pourquoi, puis la suite.** Exemple : « Dégradé : {skill} n'a pas répondu. L'étape continue sans lui. »
5. **Le nom du décideur apparaît sur la carte de décision et dans le fichier de décision, nulle part ailleurs.** Le récit écrit « la personne nommée » (A5 du document 01).
6. **Typographie française stricte** : espace insécable avant `:` `;` `?` `!` et dans les guillemets. Pas de point médian.

### Ouverture et contrat de séance

| N° | Contexte | Texte |
| --- | --- | --- |
| T-01 | Question 1 | Combien de temps avant la décision ? Rush (pressé) ou Pace (dans les temps). |
| T-02 | Question 2 | Qu'avez-vous en main : une hypothèse (Assumption, confiance faible), des signaux (Signal, confiance moyenne) ou une preuve (Evidence, confiance haute) ? |
| T-03 | Confirmation | Mode : {mode}. État : {état}. Je prépare les critères. |
| T-04 | Contrat | Voici ce qui sera suffisant pour {étape}. Corrigez un critère si besoin : je réécris la liste. |
| T-05 | Tous les critères cochés | Suffisant. Je m'arrête ici. |
| T-06 | Changement de contexte | {Le mode / L'état} passe de {ancien} à {nouveau}. Je réécris les critères de {étape}. |
| T-07 | Relance | {sous-agent} s'était arrêté : critère non atteint, {critère}. Je reprends. |

### Étapes, rail et champs manquants

| N° | Contexte | Texte |
| --- | --- | --- |
| T-08 | Rail, en attente | {étape} : en attente |
| T-09 | Rail, en cours | {étape} : en cours |
| T-10 | Rail, terminé | {étape} : terminée |
| T-11 | Rail, à reprendre | {étape} : à reprendre, critères non atteints |
| T-12 | Rail, `choose` attend | choose : attend le décideur |
| T-13 | `sense` terminé | Problème formulé : {problème} |
| T-14 | `devise` terminé | {n} options. Chacune porte ses quatre champs et ses quatre lignes de lentille. |
| T-15 | Option demandée | Une option de plus : {n+1} au total. |
| T-16 | Champs manquants | {n} champ(s) à renseigner avant de décider : |
| T-17 | Un champ manquant | {option} – {champ} : à renseigner. Source : {source, ou « aucune »}. |

### Décision et écriture

| N° | Contexte | Texte |
| --- | --- | --- |
| T-18 | Tension | Deux effets se contredisent : {effet A} et {effet B}. La décision revient à la personne nommée. |
| T-19 | Champ | Décideur (obligatoire) |
| T-20 | Bouton inactif | Indiquez le décideur pour enregistrer la décision. |
| T-21 | Bouton actif | Enregistrer la décision |
| T-22 | Tranchée | Décision enregistrée : option {X}, par {décideur}. |
| T-23 | Demande d'écriture | Herbert souhaite écrire {fichier}. Voici la modification : |
| T-24 | Boutons | Accorder / Refuser |
| T-25 | Accordée | Écrit et journalisé. Vous pouvez l'annuler. |
| T-26 | Refusée | Refusé. Rien n'a été modifié. |

### Attestation, récit et fin de session

| N° | Contexte | Texte |
| --- | --- | --- |
| T-27 | Niveaux | Output, Outcome, Impact. États : Prouvé, Sans preuve, Contribution. |
| T-28 | Niveau sans preuve | Aucune preuve à ce jour. Mesure planifiée : {date, ou « aucune »}. |
| T-29 | Impact | Contribution du design, avec d'autres facteurs. Non attribué au design seul. |
| T-30 | Phrase sans trace | Sans trace : à retirer ou à documenter. |
| T-31 | Récit, décideur | La personne nommée a tranché pour l'option {X}. |
| T-32 | Question 1 de fin | Avez-vous retenu une option que vous n'auriez pas proposée seul ? Oui / Non |
| T-33 | Question 2 de fin | Le problème a-t-il été reformulé en cours de session ? Oui / Non |
| T-34 | Session incomplète | Session interrompue avant la revue. Les deux questions de fin ne sont pas posées. |

### Mode dégradé et cas limites

Les quatre cas limites sont ceux de l'évaluation (A11 du document 01).

| N° | Contexte | Texte |
| --- | --- | --- |
| T-35 | Dégradé | Dégradé : {skill} n'a pas répondu. {étape} continue sans lui. |
| T-36 | Bloquant | Bloquant : {skill} est indispensable et n'a pas répondu. {étape} s'arrête. |
| T-37 | Source absente | Je ne trouve pas la source de {champ}. Je le laisse à renseigner. |
| T-38 | Entrée hors sujet | Cette demande sort du périmètre d'Herbert : décider une question de design, et le prouver. Je ne la traite pas. |
| T-39 | Entrée trop longue | L'entrée dépasse ce que je peux traiter. Pouvez-vous la réduire ou indiquer la partie utile ? |
| T-40 | Cas ambigu | Je vois deux lectures possibles : {A} ou {B}. Laquelle retenir ? |
| T-41 | Pas de réponse | Je ne sais pas. {Raison}. |

### Questions ouvertes

| N° | Question | Statut |
| --- | --- | --- |
| QD-01 | Quelles durées séparent Rush de Pace ? | À renseigner : la spec définit Rush comme « pressé » et Pace comme « dans les temps », sans durée. T-01 reprend ces libellés. |

## 4. Accessibilité

Les contrastes sont mesurés sur la maquette Figma le 20 novembre 2026 : 433 nœuds de texte parcourus sur les dix pages de composants et d'écrans, 137 combinaisons distinctes de texte, de fond, de taille et de graisse. Le seuil est WCAG 2.2 AA : 4,5:1 pour le texte courant, 3:1 pour le grand texte (24 px, ou 18,66 px en gras). Les étiquettes de présentation du fichier Figma ne font pas partie du produit et sont écartées.

### Règles

| N° | Règle | Vérification |
| --- | --- | --- |
| AC-01 | Tout texte atteint 4,5:1 sur son fond, le grand texte 3:1. La teinte tertiaire passe de `#767c64` à `#656b53`. | Contraste calculé sur chaque couple de la maquette |
| AC-02 | Le texte courant fait 14 px au minimum ; les étiquettes et les métadonnées 12 px au minimum. | Relevé des tailles dans la maquette |
| AC-03 | Aucun état ne tient à la couleur seule : prouvé, sans preuve, contribution, dégradé, bloquant, accordée, refusée portent chacun un mot. | Relecture des huit composants |
| AC-04 | Un composant inactif est exempté du contraste, mais son texte dit pourquoi il est inactif (T-20). | Relecture de la carte de décision |

### Mesures : ce qui échoue

| Texte | Fond | Contraste | Où | Après correction |
| --- | --- | --- | --- | --- |
| `#767c64` | `#ffffff` | 4,34:1 | Étiquettes 11 px, sources, textes d'aide de 14 et 15 px | 5,55:1 |
| `#767c64` | `#eceee5` | 3,71:1 | Libellés « Effet tech », « Output », « Écartée », « Lien vers la preuve » | 4,74:1 |
| `#767c64` | `#edf0e1` | 3,75:1 | Le mot « vous », écran de la conversation | 4,80:1 |
| `#9aa088` | `#eceee5` | 2,31:1 | Bouton « Trancher » inactif | Exempté (AC-04) |

Les autres couples passent. Les plus bas sont l'ambre `#8a5a0a` sur `#fbefd3` (5,18:1) et le secondaire `#5a5f4b` sur `#eceee5` (5,65:1). L'encre `#3b3f31` atteint de 8,2 à 10,82:1 selon le fond.

### Tailles relevées

| Taille | Où | Règle |
| --- | --- | --- |
| 14 à 20 px | Texte courant, titres de carte | Conforme à AC-02 |
| 11 px | Étiquettes, sources, métadonnées | À passer à 12 px |
| 10 px | Étiquettes de sous-agent du rail, glyphes `+` et `−` du diff, chemins de fichier, références de trace | À passer à 12 px |

### Écarts à corriger dans la maquette

La maquette n'est pas modifiée à ce jour.

1. **Teinte tertiaire** : `#767c64` devient `#656b53` partout où elle est utilisée.
2. **Tailles** : les textes de 10 et 11 px passent à 12 px.
3. **Textes** : certains libellés de la maquette diffèrent de la section 3, qui fait foi. « Suffisant. J'arrête ici. » devient T-05, « Trancher » devient T-21, « Herbert veut modifier un fichier » devient T-23.

### Hors périmètre

Le focus clavier et la taille des cibles ne sont pas mesurés : `alpha.1` est en conversation, sans composant interactif propre. Ils entrent dans la spec du panneau (C-10) et de la fenêtre flottante (C-12).

## 5. `alpha.1` et suite

`alpha.1` se joue en conversation, dans Claude Code seul. Chaque composant de la section 2 y est rendu en bloc de texte, avec les textes de la section 3. Tout ajout d'interface va au registre.

### Dedans, dehors

| Moment | Dans `alpha.1` | Au registre |
| --- | --- | --- |
| Ouverture | Deux questions, une réponse chacune | – |
| Contrat de séance | Les critères écrits par Herbert, corrigeables | Critères cochés en direct (C-08) |
| Avancement | Une ligne de statut par étape | Fil des quatre étapes avec leur état (C-08) |
| Options | Un bloc par option | Cartes côte à côte (C-10) |
| Champs manquants | Les « À renseigner » listés | Remplis sur place, avec leur source (C-09) |
| Décision | Le décideur obligatoire, la tension entre deux effets (G-04) | Carte de décision dans un panneau (C-10) |
| Écritures | Une validation avant toute modification (G-05) | Validation façon diff dans le panneau (C-10) |
| Attestation | Les trois niveaux, ceux sans preuve marqués comme tels | – |
| Récit | Une référence de trace par phrase | Trace au survol (C-10) |
| Mode dégradé | Un message qui dit quel skill a échoué | – |
| Contexte et exécution | – | Fenêtre flottante : contexte capturé à la validation humaine, états d'exécution en direct (C-12) |
| Autres environnements | – | Hors Claude Code, dont Cowork (C-11) |

La maquette Figma montre la cible (panneau, cartes). Aucun des neuf composants n'est rendu en carte graphique dans `alpha.1` : la maquette sert de référence pour le panneau (C-10), pas de modèle d'implémentation du premier test.

### Critères d'acceptation

| N° | Critère | Vérification |
| --- | --- | --- |
| D1 | Une session de test complète traverse les neuf moments de la section 1, dans l'ordre | Relecture de la session contre le tableau |
| D2 | Chaque message émis reprend le texte de son gabarit T-01 à T-41, valeurs remplies | Comparaison par script. À confirmer avec le document 03 : elle suppose que l'orchestrateur sorte les messages dans une forme fixe. |
| D3 | Les règles AC-01 à AC-04 sont appliquées à la maquette, puis les contrastes sont recalculés | Nouveau relevé des contrastes dans Figma. Le travail dans Figma reste à faire, avec l'accord du designer. |
| D4 | Aucun élément du registre (C-08 à C-12) n'apparaît dans une session d'`alpha.1` | Relecture de la session |

### Questions ouvertes

| N° | Question | Statut |
| --- | --- | --- |
| QD-01 | Quelles durées séparent Rush de Pace ? | À renseigner (section 3) |
| QD-02 | Quel rang de réintroduction pour C-08, C-09 et C-10 ? | À fixer avant la fin du gel (registre : « à classer ») |
