# Distribution des naissances et du genre dans le temps

# Explore available data

Count available triples

    SELECT (COUNT(*) as ?n)
    WHERE {
        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
            {?s ?p ?o}
    }

Résultat : 40380

Inspect the labels

    ### 
    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    SELECT (COUNT(*) as ?n)
    WHERE {
    
    SELECT (COUNT(*) as ?n)
    WHERE {
        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
            {?s a wd:Q5;
                rdfs:label ?label}
    }
    GROUP BY ?s
    HAVING (?n > 1)
    }

Number of persons with more than one label : 78

# Explore the gender

Group by and count genders

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    SELECT ?genLabel (COUNT(*) as ?n)
    WHERE {
        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
    {
        ?s a wd:Q5.   
        OPTIONAL {
        ?s wdt:P21 ?gen.
        ?gen rdfs:label ?genLabel
        }
    }
    }
    GROUP BY ?genLabel
    ORDER BY DESC(?n)

Résultats :

| genLabel           | n    |
| ------------------ | ---- |
| male               | 6879 |
| female             | 3111 |
| trans woman        | 3    |
| undisclosed gender | 2    |
| trans man          | 1    |
| genderqueer        | 1    |
| cisgender woman    | 1    |
| non-binary         | 1    |
| transmasculine     | 1    |

Persons with more than one gender

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>

    SELECT ?s (GROUP_CONCAT(?genLabel; separator= ' | ') AS ?labels)
    WHERE {
        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata> {
        ?s wdt:P21 ?gen.
        ?gen rdfs:label ?genLabel
    }
    } 
    GROUP BY ?s
    HAVING(COUNT(*) > 1)

Résultats : 0

# Prepare to data analyse

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>


    SELECT ?s ?label ?birthDate ?genLabel
    WHERE {
        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
            {
                ?s wdt:P21 / rdfs:label ?genLabel;
                rdfs:label ?label;
                wdt:P569 ?birthDate.
            }
    }
    ORDER BY ?birthDate
    LIMIT 10

Number of persons (with double labels, genders, etc.)

PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT (COUNT(*) as ?n)
WHERE {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
        {
          # ?s wdt:P31 wd:Q5 
          ?s a wd:Q5
          }
}

Résultats : 10000

Take one person only once : finaly query to prepare the data for analysis

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>


    SELECT  ?s (MIN(?label) as ?label) (xsd:integer(MIN(?birthDate)) as ?birthDate) (MIN(?genLabel) AS ?genLabel)
    WHERE {
        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
            {?s a wd:Q5;
                wdt:P21 ?gen;
                rdfs:label ?label;
                wdt:P569 ?birthDate.
            ?gen rdfs:label ?genLabel  
            }
    }
    GROUP BY ?s
    LIMIT 10

Number of real persons, without double lables etc.

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

    SELECT (COUNT(*) as ?n)
    WHERE {
        SELECT  ?s (MIN(?label) as ?label) (xsd:integer(MIN(?birthDate)) as ?birthDate) (MIN(?genLabel) AS ?genLabel)
        WHERE {
            GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
                {?s a wd:Q5;
                    wdt:P21 ?gen;
                    rdfs:label ?label;
                    wdt:P569 ?birthDate.
                ?gen rdfs:label ?genLabel  
                }
        }
        GROUP BY ?s
    }

Résultats : 10000

# Export the data for analysis

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>


    SELECT  ?s (MIN(?label) as ?label) (xsd:integer(MIN(?birthDate)) as ?birthDate) (MIN(?genLabel) AS ?genLabel)
    WHERE {
        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
            {?s a wd:Q5;
                wdt:P21 ?gen;
                rdfs:label ?label;
                wdt:P569 ?birthDate.
            ?gen rdfs:label ?genLabel  
            }
    }
    GROUP BY ?s

[Résultats de cette requête](Analyse/data_analysis/da1_data/birth_dates_gender.csv)