Problème observé

Le dataset contient 227 232 lignes alors que seulement 160 138 identifiants id_pdc_itinerance sont uniques.

Vérification effectuée

Analyse des répétitions de id_pdc_itinerance et comparaison des dates associées (date_maj, created_at, last_modified).

Résultat

Les lignes partageant le même id_pdc_itinerance correspondent à différentes versions d'un même point de charge et non à des doublons techniques stricts.

Options envisagées
Supprimer tous les doublons
Conserver toutes les lignes
Conserver uniquement la version la plus récente
Choix retenu

Conserver la version la plus récente de chaque point de charge.

Justification métier

Le grain métier du fichier est le point de charge identifié par id_pdc_itinerance. Les répétitions représentent l'historique des mises à jour.

Décision n°2 - Normalisation de l'encodage
Problème observé

La valeur métier "Accès libre" apparaissait sous plusieurs variantes :

Accès libre
Accčs libre
Acc�s libre
Acc¸s libre
AccÃ¨s libre
Dimension qualité concernée

Cohérence

Options envisagées
Ne rien faire
Corriger uniquement certaines variantes
Normaliser toutes les variantes
Choix retenu

Normalisation de toutes les variantes vers la valeur unique :

Accès libre
Justification métier

Toutes ces variantes représentent la même information métier et faussent les analyses de fréquence.

Résultat
Avant : 6 modalités
Après : 2 modalités
Décision n°3 - Vérification de la casse des communes
Problème observé

Le sujet demande une normalisation de la casse des communes.

Vérification effectuée

Analyse d'un échantillon de la colonne consolidated_commune.

Résultat

Les communes sont déjà homogènes :

Majuscule initiale
Accents conservés
Noms composés correctement formatés
Choix retenu

Aucune transformation appliquée.

Justification métier

Une transformation systématique risquait de dégrader des données déjà conformes.

Décision n°4 - Normalisation des dates
Problème observé

Le sujet demande une normalisation des formats de dates.

Vérification effectuée

Analyse des colonnes :

date_mise_en_service
date_maj
created_at
last_modified
Résultat

Les formats sont déjà homogènes et conformes aux standards ISO.

Choix retenu

Conversion des colonnes au type datetime.

Justification métier

Permet les calculs temporels, les contrôles qualité et les tris chronologiques.

Décision n°5 - Analyse des puissances nominales
Problème observé

Présence de valeurs :

nulles (0 kW)
très élevées (> 1000)

Exemples :

7360
22000
50000
160000
Hypothèse

Certaines puissances sont probablement exprimées en watts au lieu de kilowatts.

Options envisagées
Supprimer les lignes concernées
Convertir automatiquement toutes les valeurs
Signaler les anomalies
Choix retenu

Création de deux indicateurs qualité :

flag_puissance_watts
flag_puissance_nulle
Justification métier

Une conversion automatique pourrait modifier des données valides sans validation métier.

Résultat
975 puissances supérieures à 1000
5 282 puissances égales à 0
Décision n°6 - Imputation des codes postaux
Problème observé

La colonne consolidated_code_postal contient un grand nombre de valeurs manquantes.

Vérification effectuée

Analyse des coordonnées GPS associées aux lignes concernées.

Options envisagées
Laisser les valeurs manquantes
Imputation statistique
Réutilisation des coordonnées GPS connues
Choix retenu

Imputation à partir des coordonnées GPS identiques.

Justification métier

Lorsque plusieurs bornes partagent les mêmes coordonnées GPS, le code postal connu peut être réutilisé.

Résultat
Indicateur	Valeur
Avant	92 870
Après	79 938
Imputés	12 932
Décision n°7 - Dédoublonnage final
Problème observé

67 094 répétitions supplémentaires étaient présentes sur id_pdc_itinerance.

Vérification effectuée

Tri chronologique selon date_maj.

Choix retenu

Conserver la ligne la plus récente pour chaque point de charge.

Justification métier

La version la plus récente représente l'état actuel de la borne.

Résultat
Indicateur	Valeur
Lignes avant	227 232
Lignes après	160 138
Lignes supprimées	67 094