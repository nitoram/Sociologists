# Most frequent employer 

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT ?employer ?employerLabel (COUNT(*) as ?eff)
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
		
      ?item wdt:P108 ?employer.
        ?employer rdfs:label ?employerLabel.
        FILTER(LANG(?employerLabel) = 'en')
}  
GROUP BY ?employer ?employerLabel 
ORDER BY DESC(?eff)
LIMIT 30


| employer                                | employerLabel                                      | eff |
| --------------------------------------- | -------------------------------------------------- | --- |
| http://www.wikidata.org/entity/Q280413  | National Center for Scientific Research            | 151 |
| http://www.wikidata.org/entity/Q144488  | University of Warsaw                               | 148 |
| http://www.wikidata.org/entity/Q13371   | Harvard University                                 | 144 |
| http://www.wikidata.org/entity/Q49088   | Columbia University                                | 123 |
| http://www.wikidata.org/entity/Q131252  | University of Chicago                              | 120 |
| http://www.wikidata.org/entity/Q174710  | University of California, Los Angeles              | 101 |
| http://www.wikidata.org/entity/Q174570  | London School of Economics and Political Science   | 98  |
| http://www.wikidata.org/entity/Q168756  | University of California, Berkeley                 | 95  |
| http://www.wikidata.org/entity/Q273518  | School for Advanced Studies in the Social Sciences | 89  |
| http://www.wikidata.org/entity/Q1067935 | Laval University                                   | 85  |
| http://www.wikidata.org/entity/Q230492  | University of Michigan                             | 82  |
| http://www.wikidata.org/entity/Q153006  | Freie Universität Berlin                           | 78  |
| http://www.wikidata.org/entity/Q50662   | Goethe University Frankfurt                        | 72  |
| http://www.wikidata.org/entity/Q152087  | Humboldt-Universität zu Berlin                     | 71  |
| http://www.wikidata.org/entity/Q7842    | University of Tokyo                                | 69  |
| http://www.wikidata.org/entity/Q41506   | Stanford University                                | 66  |
| http://www.wikidata.org/entity/Q49210   | New York University                                | 63  |
| http://www.wikidata.org/entity/Q49112   | Yale University                                    | 61  |
| http://www.wikidata.org/entity/Q165980  | University of Vienna                               | 55  |
| http://www.wikidata.org/entity/Q392189  | Université de Montréal                             | 55  |
| http://www.wikidata.org/entity/Q214341  | University of Amsterdam                            | 54  |
| http://www.wikidata.org/entity/Q309350  | Northwestern University                            | 54  |
| http://www.wikidata.org/entity/Q49115   | Cornell University                                 | 53  |
| http://www.wikidata.org/entity/Q34433   | University of Oxford                               | 52  |
| http://www.wikidata.org/entity/Q156725  | University of Hamburg                              | 51  |
| http://www.wikidata.org/entity/Q189441  | Jagiellonian University                            | 50  |
| http://www.wikidata.org/entity/Q859363  | Sciences Po                                        | 50  |
| http://www.wikidata.org/entity/Q49117   | University of Pennsylvania                         | 49  |
| http://www.wikidata.org/entity/Q838330  | University of Wisconsin–Madison                    | 49  |
| http://www.wikidata.org/entity/Q230899  | University of Manchester                           | 48  |

# Inspect the employer classes

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT ?class ?classLabel (COUNT(*) as ?eff)
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
		
      ?item wdt:P108 ?employer.
        ?employer rdfs:label ?employerLabel.
        FILTER(LANG(?employerLabel) = 'en')
		?employer wdt:P31 ?class.
        ?class rdfs:label ?classLabel.
        FILTER(LANG(?classLabel) = 'en')
}  
GROUP BY ?class ?classLabel 
ORDER BY DESC(?eff)
LIMIT 100

| class                                     | classLabel                                                                                          | eff  |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------- | ---- |
| http://www.wikidata.org/entity/Q875538    | public university                                                                                   | 3951 |
| http://www.wikidata.org/entity/Q3918      | university                                                                                          | 3324 |
| http://www.wikidata.org/entity/Q45400320  | open-access publisher                                                                               | 3260 |
| http://www.wikidata.org/entity/Q62078547  | public research university                                                                          | 2254 |
| http://www.wikidata.org/entity/Q902104    | private university                                                                                  | 1524 |
| http://www.wikidata.org/entity/Q23002054  | private not-for-profit educational institution                                                      | 1456 |
| http://www.wikidata.org/entity/Q43229     | organization                                                                                        | 1343 |
| http://www.wikidata.org/entity/Q23002039  | public educational institution of the United States                                                 | 1298 |
| http://www.wikidata.org/entity/Q15936437  | research university                                                                                 | 1178 |
| http://www.wikidata.org/entity/Q1767829   | comprehensive university                                                                            | 1009 |
| http://www.wikidata.org/entity/Q31855     | research institute                                                                                  | 891  |
| http://www.wikidata.org/entity/Q5341295   | educational organization                                                                            | 883  |
| http://www.wikidata.org/entity/Q38723     | higher education institution                                                                        | 832  |
| http://www.wikidata.org/entity/Q96888669  | academic publisher                                                                                  | 794  |
| http://www.wikidata.org/entity/Q615150    | land-grant university                                                                               | 585  |
| http://www.wikidata.org/entity/Q1188663   | Colonial Colleges                                                                                   | 474  |
| http://www.wikidata.org/entity/Q163740    | nonprofit organization                                                                              | 463  |
| http://www.wikidata.org/entity/Q3551775   | university in France                                                                                | 443  |
| http://www.wikidata.org/entity/Q708676    | charitable organization                                                                             | 403  |
| http://www.wikidata.org/entity/Q115427560 | University of Excellence                                                                            | 396  |
| http://www.wikidata.org/entity/Q2385804   | educational institution                                                                             | 379  |
| http://www.wikidata.org/entity/Q2085381   | publishing house                                                                                    | 352  |
| http://www.wikidata.org/entity/Q641347    | local Internet registry                                                                             | 281  |
| http://www.wikidata.org/entity/Q265662    | national university                                                                                 | 267  |
| http://www.wikidata.org/entity/Q5028741   | campus university                                                                                   | 246  |
| http://www.wikidata.org/entity/Q209465    | campus                                                                                              | 240  |
| http://www.wikidata.org/entity/Q3551519   | university in Quebec                                                                                | 210  |
| http://www.wikidata.org/entity/Q557206    | Catholic university                                                                                 | 194  |
| http://www.wikidata.org/entity/Q3914      | school                                                                                              | 183  |
| http://www.wikidata.org/entity/Q1371037   | institute of technology                                                                             | 165  |
| http://www.wikidata.org/entity/Q13582798  | French public establishment of a scientific and technological character                             | 157  |
| http://www.wikidata.org/entity/Q11396960  | production company                                                                                  | 154  |
| http://www.wikidata.org/entity/Q3354859   | collegiate university                                                                               | 140  |
| http://www.wikidata.org/entity/Q180958    | faculty                                                                                             | 131  |
| http://www.wikidata.org/entity/Q68295960  | Swedish government agency                                                                           | 126  |
| http://www.wikidata.org/entity/Q108935461 | Czech research institution                                                                          | 122  |
| http://www.wikidata.org/entity/Q4671277   | academic institution                                                                                | 117  |
| http://www.wikidata.org/entity/Q1664720   | institute                                                                                           | 105  |
| http://www.wikidata.org/entity/Q1365560   | university of applied sciences                                                                      | 103  |
| http://www.wikidata.org/entity/Q1542938   | grand établissement                                                                                 | 100  |
| http://www.wikidata.org/entity/Q11422631  | United Nations Depository Library                                                                   | 98   |
| http://www.wikidata.org/entity/Q166118    | archives                                                                                            | 92   |
| http://www.wikidata.org/entity/Q1143635   | business school                                                                                     | 92   |
| http://www.wikidata.org/entity/Q189004    | college                                                                                             | 92   |
| http://www.wikidata.org/entity/Q847027    | grande école                                                                                        | 88   |
| http://www.wikidata.org/entity/Q1075106   | state university system                                                                             | 85   |
| http://www.wikidata.org/entity/Q13226383  | facility                                                                                            | 84   |
| http://www.wikidata.org/entity/Q327333    | government agency                                                                                   | 76   |
| http://www.wikidata.org/entity/Q108403040 | university in Ontario                                                                               | 75   |
| http://www.wikidata.org/entity/Q4830453   | business                                                                                            | 72   |
| http://www.wikidata.org/entity/Q62079009  | constituent college                                                                                 | 66   |
| http://www.wikidata.org/entity/Q5420001   | exempt charity                                                                                      | 66   |
| http://www.wikidata.org/entity/Q4201890   | Institute of the Russian Academy of Science                                                         | 63   |
| http://www.wikidata.org/entity/Q7638697   | sun grant institution                                                                               | 62   |
| http://www.wikidata.org/entity/Q11057861  | autonomous university                                                                               | 62   |
| http://www.wikidata.org/entity/Q1377182   | liberal arts college                                                                                | 61   |
| http://www.wikidata.org/entity/Q2120466   | pontifical university                                                                               | 59   |
| http://www.wikidata.org/entity/Q19844914  | university building                                                                                 | 56   |
| http://www.wikidata.org/entity/Q23002037  | public educational institution                                                                      | 53   |
| http://www.wikidata.org/entity/Q21032617  | sea grant institution                                                                               | 51   |
| http://www.wikidata.org/entity/Q3660535   | women's college                                                                                     | 48   |
| http://www.wikidata.org/entity/Q4315006   | national research university                                                                        | 47   |
| http://www.wikidata.org/entity/Q11032     | newspaper                                                                                           | 46   |
| http://www.wikidata.org/entity/Q98658352  | higher education institution directly under Ministry of Education of the People’s Republic of China | 46   |
| http://www.wikidata.org/entity/Q15893266  | former entity                                                                                       | 44   |
| http://www.wikidata.org/entity/Q414147    | academy of sciences                                                                                 | 44   |
| http://www.wikidata.org/entity/Q21032622  | space grant institution                                                                             | 43   |
| http://www.wikidata.org/entity/Q1110794   | daily newspaper                                                                                     | 42   |
| http://www.wikidata.org/entity/Q155271    | think tank                                                                                          | 42   |
| http://www.wikidata.org/entity/Q7315155   | research center                                                                                     | 41   |
| http://www.wikidata.org/entity/Q16077796  | vice-ministerial level university                                                                   | 40   |
| http://www.wikidata.org/entity/Q2467461   | academic department                                                                                 | 39   |
| http://www.wikidata.org/entity/Q134016126 | state agency of New York                                                                            | 37   |
| http://www.wikidata.org/entity/Q1743327   | church college                                                                                      | 37   |
| http://www.wikidata.org/entity/Q1620945   | historically black college or university                                                            | 36   |
| http://www.wikidata.org/entity/Q2349320   | Stiftungshochschule                                                                                 | 34   |
| http://www.wikidata.org/entity/Q7075      | library                                                                                             | 33   |
| http://www.wikidata.org/entity/Q7900184   | urban university                                                                                    | 33   |
| http://www.wikidata.org/entity/Q41298     | magazine                                                                                            | 32   |
| http://www.wikidata.org/entity/Q110225820 | Jesuit university                                                                                   | 32   |
| http://www.wikidata.org/entity/Q6881511   | enterprise                                                                                          | 30   |
| http://www.wikidata.org/entity/Q6040928   | Institute of Technology                                                                             | 28   |
| http://www.wikidata.org/entity/Q33506     | museum                                                                                              | 28   |
| http://www.wikidata.org/entity/Q498162    | census-designated place in the United States                                                        | 28   |
| http://www.wikidata.org/entity/Q483242    | laboratory                                                                                          | 28   |
| http://www.wikidata.org/entity/Q55647434  | Ottoman association                                                                                 | 27   |
| http://www.wikidata.org/entity/Q221096    | public–private partnership                                                                          | 27   |
| http://www.wikidata.org/entity/Q1966910   | national academy                                                                                    | 26   |
| http://www.wikidata.org/entity/Q2667285   | ancient university                                                                                  | 26   |
| http://www.wikidata.org/entity/Q484652    | international organization                                                                          | 25   |
| http://www.wikidata.org/entity/Q15407956  | university college                                                                                  | 25   |
| http://www.wikidata.org/entity/Q108402759 | university in British Columbia                                                                      | 25   |
| http://www.wikidata.org/entity/Q2065736   | cultural property                                                                                   | 24   |
| http://www.wikidata.org/entity/Q157031    | foundation                                                                                          | 24   |
| http://www.wikidata.org/entity/Q891723    | public company                                                                                      | 24   |
| http://www.wikidata.org/entity/Q1320047   | book publisher                                                                                      | 24   |
| http://www.wikidata.org/entity/Q3550864   | French UMR                                                                                          | 23   |
| http://www.wikidata.org/entity/Q1002697   | periodical                                                                                          | 23   |
| http://www.wikidata.org/entity/Q6866556   | ministry of Poland                                                                                  | 23   |
| http://www.wikidata.org/entity/Q845392    | polytechnic                                                                                         | 22   |

# Examples of private companies

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT ?employer ?employerLabel ?classLabel (COUNT(*) as ?eff)
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
		
      ?item wdt:P108 ?employer.
        ?employer rdfs:label ?employerLabel.
        FILTER(LANG(?employerLabel) = 'en')
		?employer wdt:P31 ?class.
        ?class rdfs:label ?classLabel.
        FILTER(LANG(?classLabel) = 'en')
		FILTER regex(?classLabel, '.*business.*|.*enterprise.*|.*company.*') 
}  
GROUP BY ?employer ?employerLabel ?class ?classLabel 
ORDER BY DESC(?eff) ?employer 
LIMIT 20

| employer                                  | employerLabel                  | classLabel         | eff |
| ----------------------------------------- | ------------------------------ | ------------------ | --- |
| http://www.wikidata.org/entity/Q49112     | Yale University                | production company | 61  |
| http://www.wikidata.org/entity/Q238101    | University of Minnesota        | production company | 24  |
| http://www.wikidata.org/entity/Q127990    | Australian National University | production company | 19  |
| http://www.wikidata.org/entity/Q703620    | Copenhagen Business School     | business school    | 16  |
| http://www.wikidata.org/entity/Q598841    | Monash University              | production company | 15  |
| http://www.wikidata.org/entity/Q734764    | University of New South Wales  | production company | 13  |
| http://www.wikidata.org/entity/Q332498    | Brigham Young University       | production company | 10  |
| http://www.wikidata.org/entity/Q1057890   | RMIT University                | production company | 8   |
| http://www.wikidata.org/entity/Q1394594   | SGH Warsaw School of Economics | business school    | 8   |
| http://www.wikidata.org/entity/Q273535    | HEC Paris                      | business school    | 6   |
| http://www.wikidata.org/entity/Q1574858   | Handelshochschule Berlin       | business school    | 5   |
| http://www.wikidata.org/entity/Q658192    | Vilnius University             | business           | 5   |
| http://www.wikidata.org/entity/Q1329269   | The Wharton School             | business school    | 4   |
| http://www.wikidata.org/entity/Q142740    | MIT Sloan School of Management | business school    | 4   |
| http://www.wikidata.org/entity/Q1756541   | Vytautas Magnus University     | business           | 4   |
| http://www.wikidata.org/entity/Q2822255   | Kozminski University           | business school    | 4   |
| http://www.wikidata.org/entity/Q1061504   | Stockholm School of Economics  | business school    | 3   |
| http://www.wikidata.org/entity/Q114878562 | Analysis & Numbers             | consulting company | 3   |
| http://www.wikidata.org/entity/Q1481050   | London Business School         | business school    | 3   |
| http://www.wikidata.org/entity/Q273527    | HEC Montréal                   | business school    | 3   |