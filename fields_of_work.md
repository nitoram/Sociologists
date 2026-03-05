# Most frequent associated fields of work
Ici nous cherchons à voir quelles sont les autres occupations et domaines d'activités les plus associés à la carrière de sociologue. Si on constate que la sociologie est effectviement la discipline la plus commune dans laquelle travaillent les sociologues, on peut également noter plusieurs autres domaines d'activités affiliés tel que : la science politique, la philosophie, l'histoire et les études genre.

# Requête
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
	
        ?item wdt:P101 ?object.
        ?object rdfs:label ?objectLabel.
        FILTER(LANG(?objectLabel) = 'en')
    }  
    GROUP BY ?object ?objectLabel 
    ORDER BY DESC(?eff)
    LIMIT 100

# Résultats
| object                                    | objectLabel                       | eff  |
| ----------------------------------------- | --------------------------------- | ---- |
| http://www.wikidata.org/entity/Q21201     | sociology                         | 4596 |
| http://www.wikidata.org/entity/Q36442     | political science                 | 523  |
| http://www.wikidata.org/entity/Q5891      | philosophy                        | 410  |
| http://www.wikidata.org/entity/Q461659    | sociology of religion             | 297  |
| http://www.wikidata.org/entity/Q309       | history                           | 295  |
| http://www.wikidata.org/entity/Q1662673   | gender studies                    | 233  |
| http://www.wikidata.org/entity/Q8134      | economics                         | 232  |
| http://www.wikidata.org/entity/Q1570681   | sociology of culture              | 209  |
| http://www.wikidata.org/entity/Q23404     | anthropology                      | 197  |
| http://www.wikidata.org/entity/Q9418      | psychology                        | 169  |
| http://www.wikidata.org/entity/Q11030     | journalism                        | 168  |
| http://www.wikidata.org/entity/Q828395    | social policy                     | 164  |
| http://www.wikidata.org/entity/Q745692    | political sociology               | 162  |
| http://www.wikidata.org/entity/Q7163      | politics                          | 156  |
| http://www.wikidata.org/entity/Q12336277  | social research                   | 152  |
| http://www.wikidata.org/entity/Q48277     | gender                            | 149  |
| http://www.wikidata.org/entity/Q161733    | criminology                       | 138  |
| http://www.wikidata.org/entity/Q156035    | opinion journalism                | 132  |
| http://www.wikidata.org/entity/Q7922      | pedagogy                          | 130  |
| http://www.wikidata.org/entity/Q161272    | social psychology                 | 128  |
| http://www.wikidata.org/entity/Q8434      | education                         | 126  |
| http://www.wikidata.org/entity/Q34749     | social science                    | 125  |
| http://www.wikidata.org/entity/Q7748      | law                               | 116  |
| http://www.wikidata.org/entity/Q205398    | social work                       | 106  |
| http://www.wikidata.org/entity/Q5431887   | social inequality                 | 104  |
| http://www.wikidata.org/entity/Q7252      | feminism                          | 104  |
| http://www.wikidata.org/entity/Q597680    | urban sociology                   | 103  |
| http://www.wikidata.org/entity/Q29051     | social anthropology               | 98   |
| http://www.wikidata.org/entity/Q37732     | demography                        | 93   |
| http://www.wikidata.org/entity/Q829367    | sociology of education            | 91   |
| http://www.wikidata.org/entity/Q177626    | human migration                   | 83   |
| http://www.wikidata.org/entity/Q2290557   | sociology of the family           | 83   |
| http://www.wikidata.org/entity/Q8242      | literature                        | 82   |
| http://www.wikidata.org/entity/Q610859    | economic sociology                | 79   |
| http://www.wikidata.org/entity/Q1620918   | historical sociology              | 73   |
| http://www.wikidata.org/entity/Q15783860  | sociology of work                 | 72   |
| http://www.wikidata.org/entity/Q166542    | international relations           | 72   |
| http://www.wikidata.org/entity/Q28598     | cultural anthropology             | 70   |
| http://www.wikidata.org/entity/Q49773     | social movement                   | 69   |
| http://www.wikidata.org/entity/Q132151    | ethnography                       | 66   |
| http://www.wikidata.org/entity/Q11033     | mass media                        | 64   |
| http://www.wikidata.org/entity/Q43455     | ethnology                         | 64   |
| http://www.wikidata.org/entity/Q1418771   | culturology                       | 62   |
| http://www.wikidata.org/entity/Q34187     | religious studies                 | 62   |
| http://www.wikidata.org/entity/Q1377163   | sociological theory               | 61   |
| http://www.wikidata.org/entity/Q847034    | sociology of law                  | 61   |
| http://www.wikidata.org/entity/Q396246    | rural sociology                   | 59   |
| http://www.wikidata.org/entity/Q113209507 | creative and professional writing | 57   |
| http://www.wikidata.org/entity/Q482       | poetry                            | 56   |
| http://www.wikidata.org/entity/Q34178     | theology                          | 54   |
| http://www.wikidata.org/entity/Q9465      | ethics                            | 52   |
| http://www.wikidata.org/entity/Q7181      | globalization                     | 49   |
| http://www.wikidata.org/entity/Q1916660   | medical sociology                 | 48   |
| http://www.wikidata.org/entity/Q8458      | human rights                      | 48   |
| http://www.wikidata.org/entity/Q594322    | sociology of sport                | 48   |
| http://www.wikidata.org/entity/Q11024     | communication                     | 46   |
| http://www.wikidata.org/entity/Q2167019   | environmental sociology           | 46   |
| http://www.wikidata.org/entity/Q908604    | social history                    | 45   |
| http://www.wikidata.org/entity/Q166153    | German studies                    | 43   |
| http://www.wikidata.org/entity/Q59950     | urbanism                          | 43   |
| http://www.wikidata.org/entity/Q9174      | religion                          | 43   |
| http://www.wikidata.org/entity/Q9339103   | sociology of organisations        | 43   |
| http://www.wikidata.org/entity/Q5404323   | ethnicity                         | 43   |
| http://www.wikidata.org/entity/Q431005    | sociology of youth                | 42   |
| http://www.wikidata.org/entity/Q179805    | political philosophy              | 41   |
| http://www.wikidata.org/entity/Q2920921   | management                        | 41   |
| http://www.wikidata.org/entity/Q853710    | mass communication                | 41   |
| http://www.wikidata.org/entity/Q180592    | social philosophy                 | 40   |
| http://www.wikidata.org/entity/Q2715623   | social network                    | 40   |
| http://www.wikidata.org/entity/Q858517    | cultural history                  | 39   |
| http://www.wikidata.org/entity/Q137452860 | cultural history                  | 38   |
| http://www.wikidata.org/entity/Q3043188   | sexuality                         | 38   |
| http://www.wikidata.org/entity/Q841628    | social stratification             | 38   |
| http://www.wikidata.org/entity/Q3220391   | social networking service         | 37   |
| http://www.wikidata.org/entity/Q932233    | history of sociology              | 37   |
| http://www.wikidata.org/entity/Q1143546   | cultural studies                  | 36   |
| http://www.wikidata.org/entity/Q981008    | sociology of scientific knowledge | 36   |
| http://www.wikidata.org/entity/Q11042     | culture                           | 35   |
| http://www.wikidata.org/entity/Q115160290 | literary activity                 | 34   |
| http://www.wikidata.org/entity/Q12142141  | political activity                | 34   |
| http://www.wikidata.org/entity/Q12483     | statistics                        | 33   |
| http://www.wikidata.org/entity/Q8461      | racism                            | 32   |
| http://www.wikidata.org/entity/Q546113    | public policy                     | 32   |
| http://www.wikidata.org/entity/Q189603    | public health                     | 31   |
| http://www.wikidata.org/entity/Q8162      | linguistics                       | 31   |
| http://www.wikidata.org/entity/Q58854     | literary criticism                | 31   |
| http://www.wikidata.org/entity/Q115160303 | translating activity              | 29   |
| http://www.wikidata.org/entity/Q11936732  | international migration           | 29   |
| http://www.wikidata.org/entity/Q50637     | art history                       | 29   |
| http://www.wikidata.org/entity/Q7553      | translation                       | 29   |
| http://www.wikidata.org/entity/Q131201    | sustainable development           | 28   |
| http://www.wikidata.org/entity/Q190656    | multiculturalism                  | 28   |
| http://www.wikidata.org/entity/Q6235      | nationalism                       | 28   |
| http://www.wikidata.org/entity/Q6701907   | social communication              | 28   |
| http://www.wikidata.org/entity/Q131288    | immigration                       | 27   |
| http://www.wikidata.org/entity/Q305186    | labor market                      | 27   |
| http://www.wikidata.org/entity/Q59115     | philosophy of science             | 27   |
| http://www.wikidata.org/entity/Q115161685 | economics and politics            | 26   |
| http://www.wikidata.org/entity/Q208217    | literary studies                  | 26   |
| http://www.wikidata.org/entity/Q194105    | editing                           | 26   |
