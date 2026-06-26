## Journal de décisions - Grille d'audit qualité

### Problème

67 094 répétitions de l'identifiant `id_pdc_itinerance`.

### Options envisagées

- Supprimer toutes les répétitions avec `drop_duplicates()`
- Conserver uniquement la version la plus récente
- Analyser le grain avant toute suppression

### Choix retenu

Analyser le grain avant toute suppression.

### Justification métier

L'étude d'un identifiant répété a montré que les lignes ne sont pas identiques et peuvent correspondre à plusieurs versions ou livraisons d'un même point de charge.

Le sujet indique explicitement que le grain du fichier est potentiellement « point de charge × livraison ». Une suppression automatique risquerait donc de supprimer des données valides.
