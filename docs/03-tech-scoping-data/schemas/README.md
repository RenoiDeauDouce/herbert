# Schémas du contrat public

**Statut** : champs définis dans la [section 2 du document 03](../spec.md) ; fichiers JSON Schema à écrire. Versionnés en `0.0.0`, puis suivent la règle de versioning d'Herbert : tout changement du contrat public change la version.

## Fichiers prévus

| Schéma | Produit par | Contenu |
| --- | --- | --- |
| `problem` | `sense` | Le problème formulé : qui, quel blocage, quelle preuve |
| `option` | `devise` | Une option avec la lentille, ses états, ses cas limites et ses quatre champs fermés |
| `decision` | `choose` | La décision, sa justification, le décideur nommé, le handoff |
| `attest_card` | `review` | Output, outcome, impact et leurs preuves |
| `journal-events` | tous | Les événements du journal : identifiants, compteurs, horodatages |

Le journal ne contient aucun texte de décision ni donnée personnelle.
