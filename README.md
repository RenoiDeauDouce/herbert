# Herbert

> Herbert helps designers make good-enough decisions, and prove them.

Documents de pilotage de `0.0.1-alpha.1`. Le gel des règles court du 23 novembre 2026 à la sortie de `0.0.1`.

## Lecteur et format

- **Lecteur** : le designer qui pilote `alpha.1`, seul.
- **Format** : documents courts, des décisions et des critères d'acceptation, peu de justification.
- **Support** : Markdown, versionné avec le plugin (`0.0.1-alpha.N`).

## Documents

L'ordre suit les dépendances.

| N° | Dossier | Rôle | Dépend de | Statut |
| --- | --- | --- | --- | --- |
| 01 | [`docs/01-product-management`](docs/01-product-management/README.md) | Périmètre, critères de sortie, jalons, risques | – | À relire ([spec](docs/01-product-management/spec.md)) |
| 02 | [`docs/02-product-design`](docs/02-product-design/README.md) | Parcours, états, règles d'interface | 01 | À écrire |
| 03 | [`docs/03-tech-scoping-data`](docs/03-tech-scoping-data/README.md) | Architecture, contrat public, données, sécurité | 01 (en parallèle de 02) | À écrire |
| 04 | [`docs/04-qa-dogfooding`](docs/04-qa-dogfooding/README.md) | Tests QA, protocole et lecture du dogfooding | 01, 02, 03 | À écrire |
| – | [`docs/registre`](docs/registre/README.md) | Changements en attente pendant le gel | – | Ouvert |

## Sources de vérité actuelles, hors dépôt

- Spec d'Herbert révisée : <https://claude.ai/artifact/NU2prNBXuDHJa5BvEwKau5>
- Registre des changements (C-01 à C-12) : <https://claude.ai/artifact/Ddcdv8Y8QhkjvqU9a9uE9A>
- Maquette Figma : <https://www.figma.com/design/k498URAudOXclhXJADIkiH/Herb.ert>

Ces documents seront repris dans le dépôt à mesure que les quatre documents ci-dessus s'écrivent.

## Règles communes

- **G-01** : aucun chiffre, aucune décision ni aucune source inventés. Un champ sans source porte « À renseigner ».
- **G-04** : Herbert propose, une personne nommée décide.
- **Gel** : aucune règle ne change entre le 23 novembre 2026 et la sortie de `0.0.1`, sauf les cinq exceptions X-01 à X-05. Toute autre envie va au registre.
- **Critère d'acceptation** : un énoncé vérifiable, avec la manière de le vérifier. Pas de critère sans vérification.

## Arborescence

```text
herbert/
├── README.md
└── docs/
    ├── 01-product-management/
    ├── 02-product-design/
    ├── 03-tech-scoping-data/
    │   └── schemas/
    ├── 04-qa-dogfooding/
    └── registre/
```
