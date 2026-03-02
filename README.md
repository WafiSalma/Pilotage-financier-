# Pilotage-financier-
# Atlas Finance Insights - Pilotage Financier (2019-2025)

## Contexte
**Atlas Finance Insights** est une PME basée au Maroc qui vend des produits et services à des clients au Maroc et à l'international. Depuis 2019, l'entreprise gère un volume croissant de transactions et souhaite centraliser ses données financières pour améliorer son pilotage.

Ce projet a pour objectif de construire une solution **data end-to-end** (collecte → préparation → analyse → visualisation) permettant de suivre la performance financière sur la période 2019-2025.

## Problématique
Comment construire une solution de pilotage financier fiable et automatisée permettant de :
- Suivre la performance (ventes, dépenses, budget vs réalisé)
- Analyser les variations par catégories, segments, pays
- Intégrer un contexte macro-économique (taux de change EUR/MAD et inflation multi-pays)
- Soutenir la prise de décision stratégique

## Architecture du Projet
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│ SOURCES │────▶│ PREPARATION │────▶│ STOCKAGE │────▶│ RESTITUTION │
├─────────────────┤ ├─────────────────┤ ├─────────────────┤ ├─────────────────┤
│ • Fichiers CSV │ │ • Nettoyage │ │ • PostgreSQL │ │ • Power BI │
│ • Base SQL │ │ • Validation │ │ • Schéma étoile │ │ • Dashboard │
│ • APIs externes │ │ • Enrichissement│ │ • Indexation │ │ • Rapports │
└─────────────────┘ └─────────────────┘ └─────────────────┘ └─────────────────┘
### Sources de données
1. **Fichiers CSV** : Clients, Produits, Ventes, Dépenses, Budget, Mapping comptab
2. **APIs externes** : 
   - [Frankfurter API](https://www.frankfurter.app/) - Taux de change EUR/MAD
   - World Bank API - Inflation multi-pays

## Structure du Dépôt
atlas-finance-insights/
│
├── 📁 data/
│ ├── 📁 raw/ # Données brutes (non modifiées)
│ │ ├── client.csv
│ │ ├── produit.csv
│ │ ├── vente_facture.csv
│ │ ├── depence_charge.csv
│ │ ├── budget_mensuelles.csv
│ │ └── mapping_categories.csv
│ │
│ ├── 📁 processed/ # Données nettoyées et transformées
│ │ └── api_fx_rates.csv
│ │
│ └── 📁 api/ # Données brutes des APIs (JSON)
│ └── fx_*.json
│
├── 📁 notebooks/
│ ├── 01_eda_initial.ipynb # Analyse exploratoire initiale
│ └── 02_statistical_tests.ipynb # Tests statistiques
│
├── 📁 scripts/
│ ├── extract_api_fx.py # Extraction des taux de change
│ ├── data_quality.py # Contrôles qualité
│ ├── transform_data.py # Nettoyage et transformation
│ └── load_to_postgres.py # Chargement en base
│
├── 📁 sql/
│ ├── create_tables.sql # Création du schéma en étoile
│ ├── create_indexes.sql # Index pour optimisation
│ └── kpi_views.sql # Vues pour les KPIs
│
├── 📁 dashboard/
│ └── atlas_finance_dashboard.pbix # Fichier Power BI
│
├── 📁 docs/
│ ├── cahier_des_charges.pdf
│ └── rapport_final.pdf
│
├── 📁 config/
│ └── .env.example # Exemple de configuration
│
├── .gitignore
├── requirements.txt
├── README.md
└── LICENSE

Chargement complet
bash
# Exécution du pipeline complet
python scripts/load_to_postgres.py
Ce script exécute automatiquement :
1.	Les contrôles qualité
2.	Le nettoyage des données
3.	La transformation et l'enrichissement
4.	Le chargement en base PostgreSQL
Modélisation - Schéma en Étoile
       Dimensions
Table	Description	Clé primaire
DimDate	Calendrier (2019-2025)	date_key (AAAAMMJJ)
DimCustomer	Référentiel clients	customer_key
DimProduct	Référentiel produits	product_key
DimExpenseCategory	Catégories de dépenses	expense_cat_key
DimCountry	Pays (pour inflation)	country_key
Faits
Table	Description	Granularité
FactSales	Ventes/factures	Transaction
FactExpenses	Dépenses	Transaction
FactBudget	Budget mensuel	Mois x Catégorie
FactFXRates	Taux de change	Jour x Devise
FactInflation	Taux d'inflation	Année x Pays
KPIs et Analyses
Principaux indicateurs
KPI	Formule	Granularité
CA (EUR)	SUM(FactSales.total_eur)	Mois/Trimestre/Année
CA consolidé (MAD)	SUM(total_eur * eur_to_mad)	Mois
Dépenses totales	SUM(FactExpenses.amount_mad)	Mois
Écart budget	Dépenses - Budget	Mois x Catégorie
Taux de remise moyen	AVG(discount_rate)	Mois/Segment
Analyses clés
•	Tendances temporelles : Évolution du CA et des dépenses
•	Segmentation : Performance par pays, segment client, catégorie produit
•	Budget vs Réalisé : Identification des dépassements
•	Top contributeurs : Meilleurs clients et produits
•	Structure des coûts : Répartition Fixe/Variable
Dashboard Power BI
Le dashboard interactif comprend 5 pages principales :
1.	Vue Exécutive : Synthèse des KPIs principaux
2.	Analyse des Ventes : Par pays, segment, produit
3.	Analyse des Dépenses : Par catégorie, département, fournisseur
4.	Budget vs Réalisé : Suivi des écarts
5.	Contexte Macro : Taux de change et inflation
https://docs/dashboard_preview.png
Tests Statistiques
Hypothèse	Test	Résultat	Interprétation
H1: Les ventes du Q4 > autres trimestres	t-test	p < 0.001	✅ Saisonnalité confirmée
H2: Corrélation ventes/dépenses	Pearson	r = 0.47	✅ Corrélation modérée
H3: CA moyen Enterprise > autres	t-test	p = 0.33	❌ Pas de différence significative
H4: Type de dépense dépend du département	Chi²	p < 0.001	✅ Dépendance forte
Résultats et Recommandations
Insights clés
1.	Saisonnalité marquée : Le CA augmente significativement au Q4 (+19% en moyenne)
2.	Concentration des revenus : Top 10 clients = 22% du CA total
3.	Dépassements budgétaires : 3 catégories dépassent le budget de >100%
4.	Impact du change : La volatilité EUR/MAD impacte la marge jusqu'à 8%
Recommandations métier
1.	Marketing : Renforcer les campagnes en Q4 pour maximiser les ventes
2.	Achats : Renégocier les contrats avec les 5 principaux fournisseurs
3.	Pricing : Réviser les prix dans les pays à forte inflation
4.	Trésorerie : Optimiser les délais de paiement (moyenne 30 jours)
Contributeurs
•	Salma WAFI - Data Analyst - https://github.com/WafiSalma/Pilotage-financier- 
•	 Yassine AMMANI - Encadrant pédagogique

