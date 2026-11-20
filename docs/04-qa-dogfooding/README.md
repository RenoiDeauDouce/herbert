# 04 – QA et dogfooding

**Statut** : à écrire. Le plan ci-dessous est provisoire, à valider pendant le brainstorming.

**Rôle** : prouver qu'`alpha.1` tient, puis décider si `0.0.1` sort.

**Dépend de** : 01, 02 et 03, qu'il teste.

## Sections prévues

1. Scénarios de référence : un par combinaison mode × état (six), plus quatre cas limites (source absente, entrée hors sujet, entrée trop longue, cas ambigu).
2. Tests : déclenchement, contrat, garde-fous, revue humaine. Les contrôles par code passent d'abord ; chaque scénario est rejoué plusieurs fois (nombre « À renseigner »).
3. Protocole de dogfooding : qui, combien de sessions, quelles données le journal consigne.
4. Bilan du vendredi et lecture des KPI.
5. Décision de sortie : critères, hypothèses H-01 à H-09 à trancher, prolongation éventuelle.
