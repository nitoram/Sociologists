# Import the data into a triplestore

# Check basic proprieties

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX bd: <http://www.bigdata.com/rdf#>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    SELECT DISTINCT ?item  ?gender ?year ?itemLabel
        WHERE {
           
        SERVICE <https://query.wikidata.org/sparql>
            {
            {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201}  
          
            ?item wdt:P31 wd:Q5;
                wdt:P569 ?birthDate;
                wdt:P21 ?gender.
        BIND(year(?birthDate) as ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
    
        OPTIONAL {
	     ?item rdfs:label ?itemLabel.
        FILTER(LANG(?itemLabel) = 'en')
    }
        

        }
        }
        LIMIT 10

# Count the number of persons to import

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX bd: <http://www.bigdata.com/rdf#>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    SELECT (count(*) as ?effectif)
    WHERE {
           
        SERVICE <https://query.wikidata.org/sparql>
            {
            {?item wdt:P106 wd:Q2306091}
           UNION
            {?item wdt:P101 wd:Q21201}  
          
            ?item wdt:P31 wd:Q5;
                wdt:P569 ?birthDate;
                wdt:P21 ?gender.
        BIND(year(?birthDate) as ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
    
        OPTIONAL {
	     ?item rdfs:label ?itemLabel.
        FILTER(LANG(?itemLabel) = 'en')
    }
     
        }
        }
Résultat : 23159 le 12 mars 2026

# Preparing to import data

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX bd: <http://www.bigdata.com/rdf#>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    CONSTRUCT 
        {
           ?item wdt:P21 ?gender.
           ?item  wdt:P569 ?year.
           ?item rdfs:label ?itemLabel.
           ?item  rdf:type wd:Q5. }
        
        WHERE {
           
        SERVICE <https://query.wikidata.org/sparql>
            {
            {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201}   
          
            ?item wdt:P31 wd:Q5;
                wdt:P569 ?birthDate;
                wdt:P21 ?gender.
        BIND(year(?birthDate) as ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
    
        OPTIONAL {
	     ?item rdfs:label ?itemLabel.
        FILTER(LANG(?itemLabel) = 'en')
    }
     
        }
        }
        LIMIT 5

# Import the triples into a dedicated graph

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX bd: <http://www.bigdata.com/rdf#>
    PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

    INSERT {

        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
        {?item  wdt:P21 ?gender.
           ?item wdt:P569 ?year. 
           ?item rdfs:label ?itemLabel.           
           ?item  rdf:type wd:Q5.
           }    
    }
        WHERE {
          
        SERVICE <https://query.wikidata.org/sparql>
            {
            {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201}   
          
            ?item wdt:P31 wd:Q5;
                wdt:P569 ?birthDate;
                wdt:P21 ?gender.
        BIND(year(?birthDate) as ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
  
        OPTIONAL {
	     ?item rdfs:label ?itemLabel.
        FILTER(LANG(?itemLabel) = 'en')
    }
   
        }
        }
    ORDER BY ?item 
    OFFSET 0
    LIMIT 10000
    }

# Inspect imported data

    PREFIX wd: <http://www.wikidata.org/entity/>

    SELECT *
    WHERE {
  GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata> {
  ?item  a wd:Q5;
        ?p ?o.
        }
    }
    ORDER BY ?item ?p
    LIMIT 20

# Count imported data

    PREFIX wd: <http://www.wikidata.org/entity/>

    SELECT (COUNT(*) as ?number)
    WHERE {
  GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata> {

  ?item  a wd:Q5
        }
    }

Résultat : 18951

# Multiple dates

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>

    SELECT (COUNT(*) as ?number) ?item
    WHERE {
  GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata> {

  ?item  a wd:Q5;
   wdt:P569 ?birthDate.
        }
    }
    GROUP BY ?item
    HAVING (COUNT(*) > 1)

# Multiple genders

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    SELECT (COUNT(*) as ?number) ?item
    WHERE {
  GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata> {

  ?item  a wd:Q5;
   wdt:P21 ?gender.
        }
    }
    GROUP BY ?item
    HAVING (COUNT(*) > 1)

# Multiple labels

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    SELECT (COUNT(*) as ?number) ?item
    WHERE {
  GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata> {

  ?item  a wd:Q5;
    rdfs:label ?label.
        }
    }
    GROUP BY ?item
    HAVING (COUNT(*) > 1)

# Requête corrigée pour éviter les doublons de dates

On commence par vider le graph à l'aide de cette requête :

    CLEAR GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>

Puis on le remplit à nouveau :

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX bd: <http://www.bigdata.com/rdf#>
    PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

    INSERT {

        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
        {?item  wdt:P21 ?min_gender.
           ?item wdt:P569 ?min_year. 
           ?item rdfs:label ?min_label.  
           ?item  rdf:type wd:Q5.
           }
    }
        WHERE {
  
  			SELECT ?item (MIN(?year) as ?min_year) (MIN(?gender) as ?min_gender) (MIN(?itemLabel) as ?min_label)
  
  			WHERE {
         
        SERVICE <https://query.wikidata.org/sparql>
            {
            {?item wdt:P106 wd:Q2306091}
            UNION
            {?item wdt:P101 wd:Q21201}   
        
            ?item wdt:P31 wd:Q5;
                wdt:P569 ?birthDate;
                wdt:P21 ?gender.
        BIND(year(?birthDate) as ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2000 )
  
        OPTIONAL {
	     ?item rdfs:label ?itemLabel.
        FILTER(LANG(?itemLabel) = 'en')
    }
   
        }
        }
    GROUP BY ?item
    ORDER BY ?item 
    OFFSET 0
    LIMIT 10000
    }

# Find the persons without label

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    SELECT (COUNT(*) AS ?number)
    WHERE {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata> {
    ?item  a wd:Q5.
    MINUS { ?item rdfs:label ?label}
    }
    }

Résultat : 579

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    SELECT ?item ?itemLabel
    WHERE {
    {GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata> {
        ?item  a wd:Q5.
        MINUS { ?item rdfs:label ?label}
        }
    }
  SERVICE <https://query.wikidata.org/sparql>
    {
      ?item rdfs:label ?itemLabel.
        FILTER(LANG(?itemLabel) = 'ru')
    }
  
        }
    LIMIT 10

# Add label to the class "Person"

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    PREFIX owl: <http://www.w3.org/2002/07/owl#>

    INSERT DATA {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
    {
        wd:Q5 rdfs:label "Person".
    }
    }

# Add the gender class

    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX bd: <http://www.bigdata.com/rdf#>
    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>

    SELECT (COUNT(*) as ?n)
    WHERE
   {
   SELECT DISTINCT ?gender
   WHERE {
      GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
         {
            ?s wdt:P21 ?gender.
         }
      }
   }

    PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>

    WITH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
    INSERT {
   ?gender rdf:type wd:Q48264.
    }
    WHERE
   {
   SELECT DISTINCT ?gender
   WHERE {
         {
            ?s wdt:P21 ?gender.
         }
      }
   }

   PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    PREFIX owl: <http://www.w3.org/2002/07/owl#>

    INSERT DATA {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
    {
        wd:Q48264 rdfs:label "Gender Identity".
    }
    }

# Verifiy imported triples and add label to genders

Nombre de triplés dans le graphe

    SELECT (COUNT(*) as ?n)
    WHERE {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
        {?s ?p ?o}
    }

Résultat : 39432

Nombre de personnes avec plus d'un label

    SELECT (COUNT(*) as ?n)
    WHERE {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
        {?s rdf:label ?o}
    }
    GROUP BY ?s
    HAVING (?n > 1)

Résultat : 0

# Explore the gender

Nombre de personne ayant plus d'un genre

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>

    SELECT ?s (COUNT(*) as ?n)
    WHERE {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
        {?s wdt:P21 ?gen}
    }
    GROUP BY ?s
    HAVING (?n > 1)

Nombre de personne par genre 

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>

    SELECT ?gen (COUNT(*) as ?n)
    WHERE {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
        {?s wdt:P21 ?gen}
    }
    GROUP BY ?gen
    HAVING (?n > 1)

Nombre de personne par genre selon l'époque

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>

    SELECT ?gen (COUNT(*) as ?n)
    WHERE {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
        {?s wdt:P21 ?gen;
            wdt:P569 ?birthDate.
        FILTER (?birthDate < '1900')   
          }
    }
    GROUP BY ?gen
    HAVING (?n > 1)

Retrieve all the genders 

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX bd: <http://www.bigdata.com/rdf#>

    SELECT ?gen ?genLabel
    WHERE {

    {SELECT DISTINCT ?gen
    WHERE {
        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata> 
            {?s wdt:P21 ?gen}
    }
    }   

    SERVICE  <https://query.wikidata.org/sparql> {
   
        BIND(?gen as ?gen)
        BIND ( ?genLabel as ?genLabel)
        SERVICE wikibase:label { bd:serviceParam wikibase:language "en" }  
    }
    }

Résulat : les labels sont respectivement : female, male, trans woman, cisgender woman, unidsclosed gender, transmasculine, trans man, genderqueer, non-binary

Add label to the gender

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX bd: <http://www.bigdata.com/rdf#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    CONSTRUCT {
        ?gen rdfs:label ?genLabel
    
    } 
    WHERE {

        {SELECT DISTINCT ?gen
        WHERE {
            GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>  
                {?s wdt:P21 ?gen}
        }
        }   

        SERVICE  <https://query.wikidata.org/sparql> {
    
            BIND(?gen as ?gen)
            BIND ( ?genLabel as ?genLabel)
            SERVICE wikibase:label { bd:serviceParam wikibase:language "en" }  
        }
    }

Add label to the gender : INSERT

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX bd: <http://www.bigdata.com/rdf#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    WITH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata> 
    INSERT {
        ?gen rdfs:label ?genLabel
    
    } 
    WHERE {  

        {SELECT DISTINCT ?gen
        WHERE {
            GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>  
                {?s wdt:P21 ?gen}
        }
        }   

        SERVICE  <https://query.wikidata.org/sparql> {
    
            BIND(?gen as ?gen)
            BIND ( ?genLabel as ?genLabel)
            SERVICE wikibase:label { bd:serviceParam wikibase:language "en" }  
        }
    }

Verifiy data insertion

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX wikibase: <http://wikiba.se/ontology#>
    PREFIX bd: <http://www.bigdata.com/rdf#>

    SELECT ?gen ?genLabel ?n
    WHERE
    {
        {
        SELECT ?gen (COUNT(*) as ?n)
            WHERE {
                GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
                        {
                ?s wdt:P21 ?gen.
                }
            }  
            GROUP BY ?gen      
        }  
        ?gen rdfs:label ?genLabel
        } 

# Prepare data to analyze

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

Number of persons

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

Résultat : 10000

Personnes avec choix aléatoire de modalités pour variables doubles

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

    SELECT  ?s (MAX(?label) as ?label) (xsd:integer(MAX(?birthDate)) as ?birthDate) 
        (MAX(?gen) as ?gen) (MAX(?genLabel) AS ?genLabel)
    WHERE {
        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
            {?s wdt:P21 ?gen;
                rdfs:label ?label;
                wdt:P569 ?birthDate.
            ?gen rdfs:label ?genLabel  
            }
    }
    GROUP BY ?s
    LIMIT 10

Nombre de personnes avec propriétés de base sans doublons

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    SELECT (COUNT(*) as ?n)
    WHERE {
    SELECT  ?s (MAX(?label) as ?label) (xsd:integer(MAX(?birthDate)) as ?birthDate) 
                (MAX(?gen) as ?gen) (MAX(?genLabel) AS ?genLabel)
    WHERE {
        GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
            {?s wdt:P21 ?gen;
                rdfs:label ?label;
                wdt:P569 ?birthDate.
            }
    }
    GROUP BY ?s
    }

Résultat : 9421

Ajouter le label pour la propriété "date of birth"

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    INSERT DATA {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
    {    wdt:P569 rdfs:label "date of birth"
    }  
    }

Nombre de personnes avec propriétés de base sans doublons (choix aléatoire)

    PREFIX wd: <http://www.wikidata.org/entity/>
    PREFIX wdt: <http://www.wikidata.org/prop/direct/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

    INSERT DATA {
    GRAPH <https://nitoram.github.io/Sociologists/graphs-defs.html#wikidata>
    {    wdt:P21 rdfs:label "sex or gender"
    }  
    }