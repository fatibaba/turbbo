  # Technical evaluation of TURBBO with competency questions.
We designed a set of competency questions to ensure that the ontology covers all the concepts needed to describe a behaviour in the context of how it is measured and how it relates to another behaviour. These concepts model the data elements of the knowledge base that will contain data on studies that measure correlation between behaviours. We are building data analysis tools in the TURBBO ecosystem that will carry out metanalysis of the studies to that will give behavioural scientists more insight into how behaviours are correlated even if they were not studied or measured together. For example, if sleep and alcohol consumptions are measured in one study, and sleep and exercise are measured in a different study, the tools will give an insight to how alcohol consumption and exercise may be related. This evaluation of the ontology aims to check that the necessary data elements required by the data analysis tools are adequately represented. 

We present the competency questions in the table below and the sort of responses we expect.  

| Competency question  | Candidate answer |
| --- | --- |
| CQ1. What is the name of the study that measured/observed a named behaviour? | Name of the study |
| CQ2. What is the sample size of the study that is used to measure a named behaviour? | Sample size values |
| CQ3.  Is a named behaviour legal? | Yes, No |
| CQ4. Where does a named behaviour occur (location)? | ‘at home’, ‘at work’, ‘at the park’, ‘in a restaurant’ |
| CQ5. If a named behaviour requires the use of materials, what are the materials? | Car, walking stick, etc. |
| CQ6. Which behaviours are correlated to a named behaviour and what are the correlation coefficients r | Name(s) of correlated behaviour (eg, recycling, sleeping, walking, etc), a value of r (0.5, 0.1, etc) |
| CQ7. What is the weighted correlation between two behaviours that are measured in more than one study? | a calculated weighted correlation (Pearson, Spearman, etc) |
| CQ8. What is the association between two behaviours that have not been measured/compared in the same study? | the relationship represented as the list of behaviours that link the two behaviours. |

As an example dataset for this evaluation, we obtained data from 5 studies that have measured a range of behaviours. We added the data elements and mapped them to their corresponding classes in TURBBO. The complete dataset is available in [turbbo-dataset.ttl](https://raw.githubusercontent.com/fatibaba/turbbo/main/turbbo-dataset.ttl). We used a local installation of Apache Jena Fuseki server to run the SPARQL queries. Instructions on how to install and use the Apache Jena Fuseki server can be found [here](https://jena.apache.org/documentation/fuseki2/). We present the SPARQL queries we designed to represent the competency questions and the results we obtained by running the queries in the section below. While executing the queries, the first couple of iterations identified concepts that we did not model in the ontologies, so we went back and added those. We also identified some concepts that were complicated so we simplified the representation. For example, we initially had two object properties, relatedsBehaviourA and relatesBehaviourB that were serving the same purpose, therefore we simplified it with one relatesBehaviour object property. We also had some object properties that we had to convert to data properties to allow us recored literal values. 

## SPARQL queries and answers

1. What is the name of the study that measured/observed a named behaviour?

With this question, we are trying to find out all the studies that have reported on ‘Using Alcohol’. We designed the SPARQL query to find the name of the study and if there are associated publications and their DOIs. The result of this query shows a list of the four studies that reported on "Using Alcohol" behaviour. 

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT distinct ?studyPaper ?DOI ?paperTitle
WHERE {
  ?study rdfs:label ?studyPaper.
  
  ?study ?title ?paperTitle.
  ?title rdfs:label "docTitle".
  
  ?study ?hasDOI ?DOI.
  ?hasDOI rdfs:label "hasDOI".
 
  ?study <https://purl.org/turbbo/upper_0000155> ?observation.
  ?observation ?correlate ?correlation.
  ?correlation ?measures ?behaviour. 
  ?behaviour rdfs:label ?behaviourName.
  
  filter regex(?behaviourName, "Using Alcohol")
  }
```

Answer:
| "studyPaper" | "DOI" | "paperTitle" |
| --- | --- | --- |
| "Witkiewitz et al. (2012)" | "https://doi.org/10.1037%2Fa0025363" |  "Concurrent drinking and smoking among college students: An event-level analysis." |
| "Cameron et al. (2015)" | "https://doi.org/10.1186/s13063-015-1092-4" |  "A theory-based online health behaviour intervention for new university students (U@Uni:LifeGuide): results from a repeat randomized controlled trial" |
| "Tam et al. (2021)" | "https://doi.org/10.1080/08870446.2021.2007913" |  "Self-care behaviors drinking and smoking to cope with psychological distress during COVID-19 among Chinese college students: the role of resilience” |
| "Gupta (2021)" | "https://doi.org/10.1109/ICAICST53116.2021.9497837" |  "The accuracy of supervised machine learning algorithms in predicting cardiovascular disease" |

2. What is the sample size of the study that is used to measure a named behaviour?
We used the query below to find the sample size in all the studies that measured "Drinking Alcohol" behaviour. One particular study measured "Drinking Alcohol" behaviour, three times while comparing it to three other behaviours. 

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT distinct ?studyPaper ?sampleSize
WHERE {
  ?study rdfs:label ?studyPaper.
 
  ?study <https://purl.org/turbbo/upper_0000155> ?observation.
  ?observation ?correlate ?correlation.
  ?correlation ?measures ?behaviour. 
  ?behaviour rdfs:label ?behaviourName.
  
  ?correlation ?hasSampleSize ?sampleSize. 
  ?hasSampleSize rdfs:label "hasSampleSize".
  
  filter regex(?behaviourName, "Using Alcohol")
  }
```

Answer:
| "studyPaper" | "sampleSize" |
| --- | --- |
| "Witkiewitz et al. (2012)" | "86"^^xsd:integer |
| "Cameron et al. (2015)" | "2599"^^xsd:integer |
| "Cameron et al. (2015)" | "2592"^^xsd:integer |
| "Cameron et al. (2015)" | "2607"^^xsd:integer |
| "Tam et al. (2021)" | "1225"^^xsd:integer |
| "Gupta (2021)" | "68549"^^xsd:integer |

3. Is a named behaviour legal?

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?legality
WHERE {
  ?behaviour ?isLegal ?legality.
  ?behaviour rdfs:label ?behaviourLabel.
  ?isLegal rdfs:label "is legal".
  filter regex(?behaviourLabel, "Using Alcohol")
}
```

Answer: 
This query did not show any results as the studies measuring "Using Alcohol" behaviour did not report the legality of this behaviour. 

4. Where does the named behaviour occur (location)? 

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?settingLabel
WHERE {
  ?behaviour ?occursIn ?setting.
  ?occursIn rdfs:label "occurs in".
  ?setting rdfs:label ?settingLabel.
  ?behaviour rdfs:label ?behaviourLabel.
  filter regex(?behaviourLabel, "Using Alcohol")
}
```

Answer:
A null value was returned because the dataset did not contain any information about the location (setting). 

5. If a named behaviour requires the use of materials, what are the materials?

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?materialLabel
WHERE {
  ?behaviour ?requiresMaterial ?material.
  ?requiresMaterial rdfs:label "requires material".
  ?material rdfs:label ?materialLabel.
  ?behaviour rdfs:label ?behaviourLabel.
  filter regex(?behaviourLabel, "Using Alcohol")
}
```

Answer:
There was no response to this query. The dataset did not indicate which materials were used for this behaviour. 

6. Which behaviours are correlated to a named behaviour and what are the correlation coefficients r.
For this query, we used Moving/Exercising as the named behaviour and tried to find all the correlated behaviours and their corresponding correlation coefficients r. The results show that our dataset contains 6 different measures of the Moving/Exercising behaviours against other behaviours from different studies. The result indicates that we have all the necessary concepts in the ontology to record behaviours and their correlations which can be used by different applications to conduct further data analysis (e.g. a meta-analysis). 

```
  PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
  PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

  SELECT ?behaviourA ?correlation_r ?behaviourB
  WHERE {
    ?y rdfs:label ?behaviourA.
    ?r rdfs:label "correlation coefficient r".
    ?relates rdfs:label "correlates behaviour".
    ?rValue rdfs:label  "rValue".
    ?rexample rdf:type ?r.
    ?rexample ?relates ?y.
    ?rexample ?rValue ?correlation_r.
    ?rexample ?relates ?x.
    ?x rdfs:label ?behaviourB.
    filter regex(?behaviourA, "Moving/Exercising")
    filter(?y != ?x)
  }
```

Answer:
| "behaviourA" | "correlation_r" | "behaviourB" |
| --- | --- | --- |
| "Moving/Exercising 2" | "0.192"^^xsd:decimal |	"Eating Vegetables 1" |
| "Moving/Exercising 2" | "0.01"^^xsd:decimal | "Smoking Cigar(ette) 2" |
| "Moving/Exercising 2" | "0.69"^^xsd:decimal | "Using Alcohol 2" |
| "Moving/Exercising 1" | "0.11"^^xsd:decimal | "Using Alcohol 1" |
| "Moving/Exercising 1" | "0.163"^^xsd:decimal | "Smoking Marijuana 1" |
| "Moving/Exercising 1" | "0.125"^^xsd:decimal | "Smoking Cigar(ette) 1" |
