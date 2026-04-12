# Import Birth Places

Nous cherchons ici à importer les lieux de naissance de notre population ainsi que leur coordonées géogrpahiques, ce qui nous permettra de procéder à une exploration de la classe "places" avant d'entamer une analyse des données.

# Get birth places of the population with an English label (if it exists)

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>


    SELECT DISTINCT (?item AS ?person_uri) ?birth_place_uri (MIN(?birth_place_label) as ?place_label) 
    WHERE {
                {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201}   
    
                ?item wdt:P31 wd:Q5;
                    wdt:P569 ?birthDate;
            BIND(year(?birthDate) as ?year)
            FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
    
        ?item wdt:P19 ?birth_place_uri.
            ?birth_place_uri rdfs:label ?birth_place_label.
                    FILTER(LANG(?birth_place_label) = 'en')
    
            }
            GROUP BY ?item ?birth_place_uri

# Get the places with non English names

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    SELECT DISTINCT (?item AS ?person_uri) ?birth_place_uri (MIN(?person_name_al) as ?place_label) 
    WHERE {
                {?item wdt:P106 wd:Q2306091}
                UNION
                {?item wdt:P101 wd:Q21201}   
    
                ?item wdt:P31 wd:Q5;
                    wdt:P569 ?birthDate;
            BIND(year(?birthDate) as ?year)
            FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )   
        ?item wdt:P19 ?birth_place_uri.

        MINUS{?birth_place_uri rdfs:label ?place_label_en.
                    FILTER(LANG(?place_label_en) = 'en') }
            ?birth_place_uri rdfs:label ?person_name_al. 
            
            }
            GROUP BY ?item ?birth_place_uri

# Get the places' geo-coordinates

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>


    SELECT ?birth_place_uri (MIN(?coordinates) as ?long_lat)
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
            }
    
        ?birth_place_uri wdt:P625 ?coordinates.
    
            }
    GROUP BY ?birth_place_uri

Les données recueillies seront désormais analysées à l'aide des requêtes SQL présentes sur [cette page](Analyse/data_analysis/da2_data/da2-birth-places.sql)