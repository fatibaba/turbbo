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

As an example dataset for this evaluation, we obtained data from 6 studies that have measured a range of behaviours. We added the data elements and mapped them to their corresponding classes in TURBBO. The complete dataset is available in [turbbo-dataset.ttl](https://raw.githubusercontent.com/fatibaba/turbbo/main/turbbo-dataset.ttl). We used a local installation of Apache Jena Fuseki server to run the SPARQL queries. Instructions on how to install and use the Apache Jena Fuseki server can be found [here](https://jena.apache.org/documentation/fuseki2/). We present the SPARQL queries we designed to represent the competency questions and the results we obtained by running the queries in the section below. While executing the queries, the first couple of iterations identified concepts that we did not model in the ontologies, so we went back and added those. We also identified some concepts that were complicated so we simplified the representation. For example, we initially had two object properties, relatedsBehaviourA and relatesBehaviourB that were serving the same purpose, therefore we simplified it with one relatesBehaviour object property. We also had some object properties that we had to convert to data properties to allow us recored literal values. 

## SPARQL queries and answers

1. What is the name of the study that measured/observed a named behaviour?

With this question, we are trying to find out all the studies that have reported on ‘past recycling behaviour’. We designed the SPARQL query to find the name of the study and if there are associated publications and their DOIs. 

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX turbbo: <https://purl.org/turbbo#>

SELECT ?study ?paper ?DOI ?paperTitle
WHERE {
  ?s ?m ?b;
      rdfs:label ?study.
  ?b rdfs:label "past recycling".
  ?m rdfs:label "measures".
  
  ?s turbbo:produces ?p.
  ?p rdfs:label ?paper.
  
  ?p ?title ?paperTitle.
  ?title rdfs:label "docTitle".
  
  ?p ?hasDOI ?DOI.
  ?hasDOI rdfs:label "hasDOI".
  }
```

Answer:
```
<?xml version="1.0"?>
<sparql xmlns="http://www.w3.org/2005/sparql-results#">
  <head>
    <variable name="study"/>
    <variable name="paper"/>
    <variable name="DOI"/>
    <variable name="paperTitle"/>
  </head>
  <results>
    <result>
      <binding name="study">
        <literal>Ha and Kwon Fash Text (2016)</literal>
      </binding>
      <binding name="paper">
        <literal>Ha &amp; Kwon (2016)</literal>
      </binding>
      <binding name="DOI">
        <literal>10.1186/s40691-016-0068-7</literal>
      </binding>
      <binding name="paperTitle">
        <literal>Spillover from past recycling to green apparel shopping behavior: the role of environmental concern and anticipated guilt</literal>
      </binding>
    </result>
  </results>
</sparql>
```


6. Which behaviours are correlated to a named behaviour and what are the correlation coefficients r

```
  PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
  PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

  SELECT ?behaviourA ?correlation ?behaviourB
  WHERE {
    ?y rdfs:label ?behaviourA.
    ?r rdfs:label "correlation coefficient r".
    ?relates rdfs:label "correlates behaviour".
    ?rValue rdfs:label  "rValue".
    ?rexample rdf:type ?r.
    ?rexample ?relates ?y.
    ?rexample ?rValue ?correlation.
    ?rexample ?relates ?x.
    ?x rdfs:label ?behaviourB.
    filter regex(?behaviourA, "moving/exercise")
    filter(?y != ?x)
  }
```

Answer:
| "y" | "correlation" | "x" |
| --- | --- | --- |
| "moving/exercise 02" | "0.11" |	"using alcohol 02" |
| "moving/exercise 01" | "0.163" | "smoking marijuana 01" |
