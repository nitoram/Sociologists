# Most frequent occupations

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

SELECT ?object ?objectLabel (COUNT(*) as ?eff)
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
	
        ?item wdt:P106 ?object.
        ?object rdfs:label ?objectLabel.
        FILTER(LANG(?objectLabel) = 'en')
}  
GROUP BY ?object ?objectLabel 
ORDER BY DESC(?eff)
LIMIT 10

| object                                  | objectLabel         | eff   |
| --------------------------------------- | ------------------- | ----- |
| http://www.wikidata.org/entity/Q2306091 | sociologist         | 18372 |
| http://www.wikidata.org/entity/Q1622272 | university teacher  | 6268  |
| http://www.wikidata.org/entity/Q36180   | writer              | 1607  |
| http://www.wikidata.org/entity/Q82955   | politician          | 1204  |
| http://www.wikidata.org/entity/Q1238570 | political scientist | 1026  |
| http://www.wikidata.org/entity/Q201788  | historian           | 1014  |
| http://www.wikidata.org/entity/Q4964182 | philosopher         | 927   |
| http://www.wikidata.org/entity/Q1930187 | journalist          | 771   |
| http://www.wikidata.org/entity/Q188094  | economist           | 677   |
| http://www.wikidata.org/entity/Q4773904 | anthropologist      | 601   |
