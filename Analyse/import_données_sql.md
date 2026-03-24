# Importer les données dans une base de données SQLite

# Exporter la population depuis Wikidata

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>


    SELECT DISTINCT (?item AS ?person_uri) ?year ?gender_label ?gender_uri 
    WHERE {
                {?item wdt:P106 wd:Q2306091}
        UNION
        {?item wdt:P101 wd:Q21201}  
    
                ?item wdt:P31 wd:Q5;
                    wdt:P569 ?birthDate;

            BIND(year(?birthDate) as ?year)
            FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
    
            OPTIONAL {
                # The item can have or not a gender property
                ?item wdt:P21 ?gender_uri.
                ?gender_uri rdfs:label ?gender_label.
                FILTER(LANG(?gender_label) = 'en')
            }

        }
    ORDER BY ?item

# Préparation et inspection des données

[Voir cette page pour les requêtes SQL](Analyse/data_analysis/da1-import-population.sql)

# Importer les labels

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    SELECT DISTINCT (?item AS ?person_uri) ?person_label
    WHERE {
                {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201}   
            
                ?item wdt:P31 wd:Q5; 
                    wdt:P569 ?birthDate;
            BIND(year(?birthDate) as ?year)
            FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
                ?item rdfs:label ?person_label.
                FILTER(LANG(?person_label) = 'en')
            }
    ORDER BY ?item

# Recherche de label qui ne soient pas en anglais pour les labels manquants

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>


    SELECT DISTINCT (?item AS ?person_uri) (min(?person_label_al) as ?person_label )
    #SELECT (COUNT(*) AS ?n)
    WHERE {
                {?item wdt:P106 wd:Q2306091}
                UNION
                {?item wdt:P101 wd:Q21201}   
    
                ?item wdt:P31 wd:Q5; 
                    wdt:P569 ?birthDate;

            BIND(year(?birthDate) as ?year)
            FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
            MINUS{?item rdfs:label ?person_label_en.
                FILTER(LANG(?person_label_en) = 'en')   }
        ?item rdfs:label ?person_label_al.
            }
    GROUP BY ?item

# Inspections et préparation des données pour l'analyse des années de naissance et du genre

[Voir cette page pour les requêtes SQL]()

# Importer lieux de naissance
# Récupérer tous les lieux de naissance de notre population

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>


    SELECT DISTINCT (?item AS ?person_uri) ?birth_place_uri
    #SELECT (COUNT(*) AS ?n)
    WHERE {
                {?item wdt:P106 wd:Q2306091}
                    UNION
                    {?item wdt:P101 wd:Q21201}   
    
                ?item wdt:P31 wd:Q5; 
                    wdt:P569 ?birthDate;
            BIND(year(?birthDate) as ?year)
            FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
        
        ?item wdt:P19 ?birth_place_uri.
        
            }

# Get the places with English labels, class and coordinates

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>


    SELECT ?birth_place_uri (MIN(?birth_place_label) as ?place_label) (MIN(?coordinates) as ?long_lat) ?place_class_label ?place_class_uri
    #SELECT (COUNT(*) AS ?n)
    WHERE {  
        {SELECT DISTINCT ?birth_place_uri
            WHERE {                
                {?item wdt:P106 wd:Q2306091}
                        UNION
                        {?item wdt:P101 wd:Q21201}  
    
                ?item wdt:P31 wd:Q5; 
                    wdt:P569 ?birthDate;
            BIND(year(?birthDate) as ?year)
            FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
        
        ?item wdt:P19 ?birth_place_uri.
            }
            ORDER BY ?birth_place_uri
        
            }

        OPTIONAL {?birth_place_uri rdfs:label ?birth_place_label.
        FILTER(LANG(?birth_place_label) = 'en')
                    }
        ?birth_place_uri wdt:P625 ?coordinates.
        ?birth_place_uri wdt:P31 ?place_class_uri.
        ?place_class_uri rdfs:label ?place_class_label.
        FILTER(LANG(?place_class_label) = 'en')
        
            }
    GROUP BY ?birth_place_uri ?place_class_label ?place_class_uri 

# Inspection des données

[Voir cette page pour les requêtes SQL](Analyse/data_analysis/da2-birth-places.sql)

# Préparer les données pour l'analyse

Exécution de la requête ci-dessous dans la base de données SQLite :

    SELECT wikidata_uri, label, birth_year, gender
    FROM person
    WHERE length(gender) > 1
    ORDER BY wikidata_uri ASC;