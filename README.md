📊 Projet Data Analytics — Atlas Business Solutions


Contexte et problématique

Atlas Business Solutions est une PME marocaine (80–150 employés) spécialisée dans la distribution de produits B2B. Elle disposait de plusieurs fichiers CSV de données métier (ventes, clients, produits, dépenses, budget) mais sans outil de pilotage structuré.

La problématique centrale : Comment transformer ces données brutes en un système décisionnel fiable et interactif pour améliorer le pilotage de la performance commerciale et budgétaire ?

Sources de données (fichiers CSV)

Clients:ID, nom, segment, pays, date de création
Produits:ID, nom, catégorie, prix unitaire
Ventes & Factures:Date, facture, client, produit, quantité, remise, montant
Dépenses & Charges:Date, fournisseur, département, catégorie, montant
Budget mensuel:Budget prévu par département et catégorie
Mapping comptable:Liaison catégories → comptes comptables

Méthodologie en 3 phases

Phase 1 — Nettoyage des données (Python)
Chargement, conversion des dates, détection des valeurs manquantes et doublons, vérification de l'intégrité référentielle, contrôles métier (quantités positives, remises entre 0 et 1, cohérence des devises), enrichissement comptable via le mapping.

Phase 2 — Analyse exploratoire et statistique (Python)
Distributions, statistiques descriptives, valeurs extrêmes, corrélations, évolution temporelle des ventes et dépenses, analyse par client/produit/segment. Tests statistiques : comparaison Q4 vs autres périodes, segment Enterprise vs autres, indépendance département/type de charge.

Phase 3 — Dashboard Power BI
Import des données nettoyées, modèle de données relationnel, mesures DAX, 4 pages de tableaux de bord.

Structure du Dashboard Power BI (4 pages)

Vue globale:CA, clients, panier moyen, profit, top clients/produits, carte géographique, budget vs dépenses ,SON OBJECTIF :Vision d'ensemble

Analyse des ventes:CA par segment, catégorie, pays, évolution temporelle, quantités ,SON OBJECTIF :Comprendre les sources de revenu

Clients & Produits:Analyse Pareto, contribution, rentabilité par client ,SON OBJECTIF :Identifier les acteurs clés

Budget & Rentabilité:Budget vs réalisé, profit, marge, écarts par département ,SON OBJECTIF :Pilotage financier

Résultats clés

Croissance du chiffre d'affaires constatée

Forte dépendance à certains clients (effet Pareto)

Absence de rentabilité globale malgré les ventes

Dépassement important du budget de dépenses

Conclusion principale : le problème n'est pas dans les ventes, mais dans la maîtrise des coûts


Limites

Données statiques (pas de flux en temps réel)

Taux de change EUR/MAD fixe (ventes en EUR, dépenses en MAD)

Pas de coûts détaillés par client ou produit

Analyse uniquement descriptive, sans modèle prédictif


Recommandations

Mettre en place un contrôle budgétaire rigoureux et des alertes automatiques

Réduire les dépenses non essentielles

Prioriser les produits et clients à forte valeur

Automatiser la collecte des données

Intégrer à terme des modèles prédictifs
