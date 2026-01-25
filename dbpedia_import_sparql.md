# Importer les données de DBpedia vers une base de données SQLite
Afin de pouvoir étendre notre étude et alimenter notre base de données nous avons choisi de travailler avec les données de DBpedia. Pour ce faire nous avons créé une copie de la base de données personnelles dont nous avons ensuite vider les tables pour qu'elles puissent accueillir les données qui seront importées via une série de requêtes SPARQL qui sont détaillées ci-dessous. Nous avons ici choisi d'importer depuis DBpedia la liste des personnes qui nous intéresse, c'est-à-dire les sociologues.

# Table 'person'

# Liste des personnes à importer
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT DISTINCT ?person_uri (STR(?label) AS ?persName) ?birthYear
WHERE { 
    dbr:List_of_sociologists ?p ?person_uri.
    ?person_uri a dbo:Person;
            dbo:birthDate ?birthDate;
            rdfs:label ?label.
    BIND(xsd:integer(SUBSTR(STR(?birthDate), 1, 4)) AS ?birthYear)
    FILTER ( ?birthYear > 1770
            && LANG(?label) = 'en')
}
ORDER BY ?birthYear

# Compter les doublons éventuels
SELECT person_uri
FROM dbp_liste_personnes lp 
GROUP BY person_uri
HAVING COUNT(*) > 1 ;

# Compter les personnes
SELECT COUNT(*)
FROM dbp_liste_personnes lp;

Nous obtnons un résultat de 443 personnes.

# Insérer les données de DBpedia dans la table 'person'
INSERT INTO person (birth_year, dbpedia_uri, label, import_metadata)
  SELECT birthYear, person_uri, persname, "Importé le 25 janvier 2026 depuis le résultat d'une requête SPARQL sur DBPedia."
  FROM dbp_liste_personnes lp ;