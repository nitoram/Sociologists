# Une étude prosopographique des sociologues

Le but de ce projet est d'explorer la structure du champ discilpinaire de la sociologie selon une approche prosopographique. À terme, nous devrions pouvoir être en mesure d'avoir une meilleure compréhension de la manière dont les sociologues se positionnent dans ce champ relativement à leurs caractérisques socio-démographiques et à leur rôle au sein de différentes institutions.

# Phase exploratoire

- [Problématique et questionnement](Exploration/problématique_questionnement.md)
- [Listes de sociologues](Exploration/listes_sociologues.md)
- [Catalogue des informations](Exploration/catalogue_informations.md)

# Création de la base de données

- [Esquisse du modèle conceptuel](Exploration/esquisse_modèle_conceptuel.jpg)
- [Modèle conceptuel vectoriel](Exploration/MCD.drawio)
- [Modèle conceptuel png](Exploration/MCD.png)
- [Commentaire du modèle conceptuel](Exploration/MCD_commentaire.md)

# Base de données

- [Base de données saisie manuellement](Exploration/sociologists.db)
- [Exportation table 'person'](Exploration/person.csv)
- [Expportation table 'birth'](Exploration/birth.csv)
- [Exportation table 'area_education'](Exploration/area_education.csv)
- [Exportation table 'education'](Exploration/education.csv)
- [Exportation table 'geographical_place_type'](Exploration/geographical_place_type.csv)
- [Exportation table 'geographical_place'](Exploration/geographical_place.csv)
- [Exportation table 'occupation'](Exploration/occupation.csv)
- [Exportation table 'organisation_type'](Exploration/organisation_type.csv)
- [Exportation table 'organisation'](Exploration/organisation.csv)
- [Exportation table 'pursuit'](Exploration/pursuit.csv)

# Exploration de la base de données

- [Requêtes SQL](Exploration/requetes_sql.md)
- [Exportation des pays de naissance](Exploration/pays_naissance.csv)
- [Exportation de l'appartenance à des organsiations](Exploration/appartenance_organisations.csv)

# Importation des données de DBpedia

- [Requêtes SPARQL utilisées](Exploration/dbpedia_import_sparql.md)
- [Base de données importées de DBpedia](Exploration/sociologists_import.db)

# Exploration de Wikidata

- [Inspection de la population](Analyse/data_production/population_inspection.md)
- [Liste de propriétés sortantes de la population](Analyse/data_production/propriétés_effectifs.md)
- [Incoming proprieties](Analyse/data_production/incoming_proprieties.md)
- [Inspection de la propriété 'occupation'](Analyse/data_production/most_frequent_occupation.md)
- [Inspection de la propriété 'employer'](Analyse/data_production/most_frequent_employer.md)
- [Inspection de la propriété 'field of work](Analyse/data_production/fields_of_work.md)

# Wikidata : data production

- [Importation de la population dans un triplestore](Analyse/data_production/import_population.md)
- [Importation de la population dans une base de données SQLite](Analyse/import_données_sql.md)

# Wikidata : data analysis

- [Distribution des naissances et du genre dans le temps](Analyse/data_analysis/distribution_birth.md)