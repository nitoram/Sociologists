# Inspection de la population
Le but de cette étape est d'avoir une meilleure compréhension de notre population d'étude en découvrant quelles informations nous avons à disposition par le biais de Wikidata afin de pouvoir répondre à nos questions de recherche.

# Nombre de sociologues (occupation)

    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wd: <http://www.wikidata.org/entity/>
    SELECT (COUNT(*) as ?eff)
    WHERE {
         {?item wdt:P106 wd:Q2306091}
        ?item wdt:P31 wd:Q5
    }

 - Résultat : 22165

# Nombre de sociologue et de personnes travaillant dans la sociologie (occupation + field of work)

    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wd: <http://www.wikidata.org/entity/>
    SELECT (COUNT(*) as ?eff)
        WHERE {
     {?item wdt:P106 wd:Q2306091}
    UNION
    {?item wdt:P101 wd:Q21201}  
    ?item wdt:P31 wd:Q5
    }

- Résultat : 27734

# Nombre de personnes sans les doublons

    SELECT (COUNT(*) as ?eff)
    WHERE {
    {
        SELECT DISTINCT ?item
        WHERE {
        ?item wdt:P31 wd:Q5;
        {?item wdt:P106 wd:Q2306091}
        UNION
        {?item wdt:P101 wd:Q21201} 
            }
        }
    }

- Résultat : 23230

# Nombre de sociologues avec un filtre sur la date de naissance

    SELECT (COUNT(*) as ?eff)
    WHERE
     {
        {
        SELECT DISTINCT ?item
        WHERE {
        ?item wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000)
            {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201} 
          
            }
        }  
    } 

- Résultat : 19082

# Inspecter les individus

    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    SELECT DISTINCT ?item ?itemLabel ?year
    WHERE {
    {
        {?item wdt:P106 wd:Q2306091}
        UNION
        {?item wdt:P101 wd:Q21201} 
    }  
    ?item wdt:P31 wd:Q5; 
            wdt:P569 ?birthDate.
    BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000)
    ?item rdfs:label ?itemLabel.
    FILTER(LANG(?itemLabel) = 'en')
    }  
    LIMIT 100

# Individus avec un english label

    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    SELECT (COUNT(*) as ?eff)
    WHERE
    {
        {
        SELECT DISTINCT ?item ?itemLabel ?year
        WHERE {
        ?item wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000)
            {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201} 
        ?item rdfs:label ?itemLabel.
        FILTER(LANG(?itemLabel) = 'en')
            }
        }  
    } 

- Résultat : 18437

# Individus sans un english label

    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    SELECT ?item ?year (group_concat(?iso_lang ; separator = ',') as ?langs) (max(?itemLabel) as ?maxLabel)
    WHERE
    { 
       { SELECT DISTINCT ?item ?year
        WHERE {
        ?item wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000)
            {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201}    
        MINUS {?item rdfs:label ?itemLabel.
            FILTER(LANG(?itemLabel) = 'en')
            }
            }
        }  
        ?item rdfs:label ?itemLabel. 
		BIND(LANG(?itemLabel) as ?iso_lang)
       }
	   GROUP BY ?item ?year
	   ORDER BY ?item
	 
- Résultat : 717
   
# List of available proprieties and their numbers

    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    SELECT ?p ?propLabel ?eff ('' as ?notes)
        WHERE {
    {
        SELECT DISTINCT  ?p  (count(*) as ?eff)
        WHERE {
        ?item wdt:P31 wd:Q5; 
             wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000)
            {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201} 
        }
		GROUP BY ?p
    }
    ?prop wikibase:directClaim ?p .
    ?prop rdfs:label ?propLabel.
        FILTER(LANG(?propLabel) = 'en')
     }  
    ORDER BY DESC(?eff) 

Résultats de la requête sur cette [page]().

# Incoming proprieties

    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    SELECT ?p ?propLabel ?eff  ('' as ?notes)
    WHERE {
    {
    SELECT DISTINCT  ?p  (count(*) as ?eff)
    WHERE {
        ?item wdt:P31 wd:Q5; 
             wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000)
            {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201} 

    
			?s ?p ?item.
        }
		GROUP BY ?p
    }
    ?prop wikibase:directClaim ?p .

    ?prop rdfs:label ?propLabel.
        FILTER(LANG(?propLabel) = 'en')
    }  
    ORDER BY DESC(?eff) 

Résultats de la requête sur cette [page]().