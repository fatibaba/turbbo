# TURBBO
This is the project repository for "Tools for understanding relationships between behaviours using ontology" project. 
## About the TURBBO project
There is an increasing body of data available from studies that examine different behaviours and how they are related, for example, the relationship between recycling and shopping for green apparel. The relationships between such behaviours have traditionally been studied in isolation. However, behavioural scientists are increasingly interested in discovering new relationships that may not be as intuitive but could be determined by analysing existing data. There is a need to develop tools that will allow behavioural scientists to easily explore and understand relationships between behaviours. This project aims to develop tools for understanding relationships between behaviours using ontology. 
## RBBO
We present the **Relationships Between Behaviours Ontology (RBBO)** as one of the TURBBO tools. RBBO provides a structural model for describing behaviours and formalises characteristics and correlations between the behaviours. We co-design the ontology with behavioural scientists who are experts in the domain. This repository contains the latest version of the complete ontology in [RBBO.owl](https://github.com/fatibaba/turbbo/blob/main/RBBO.owl)

RBBO consists of the following three modules (each can be downloaded and used as a standalone ontology): 

1. [RBBO - Properties of Studies Measuring Behaviours](https://github.com/fatibaba/turbbo/blob/main/RBBO_properties_of_studies_measuring_behaviour.owl): respresents concepts describing how behaviours are measured and compared in behavioural science studies.
2. [RBBO - Behaviour](https://github.com/fatibaba/turbbo/blob/main/RBBO_Behaviours.ttl): provides a classification of different kinds of behaviours. This module of the ontology has reused and extended the behaviour taxonomy from IC-Behavior, which is currently, the broadest of behavioural taxonomies. Larsen et al.classified 250 behaviours into nine domains: learning and applying knowledge; communicating; moving/exercising; self-care behaviour; domestic life activities; interpersonal interactions and relationships; behaviour related to major life areas; behaviour related to community, social, and civic life; and mood/state changing activities and behaviour. Behaviours considered important enough to serve as the dependent variable in articles accepted for publication in top journals were extracted from a large sample of journals in nine different behavioral and social disciplines. 

3. [RBBO - Behaviour Properties](https://github.com/fatibaba/turbbo/blob/main/RBBO_behaviour_properties.owl): consists of classes and data properties which describe the characteristics of behaviours.

The GitHub repository for the TURBBO project is https://github.com/fatibaba/turbbo

We present an ontology of behaviour, which provides a structural model for describing behaviours and formalises characteristics and correlations between the behaviours.

This is the latest version of the TURBBO ontologies which consists of three levels and can be donwloaded and used as separate ontologies:
1. The upper.owl
2. the behaviours.owl
3. The behaviour_properties.owl

There is a merged version of the TURBBO ontologies which is turbbo.owl. 

This repository also contains the SPARQL queries and answers used for evaluation of the ontology see [CompetencyQuestions](https://github.com/fatibaba/turbbo/blob/main/CompetencyQuestions.md). [turbbo-dataset.ttl](https://raw.githubusercontent.com/fatibaba/turbbo/main/turbbo-dataset.ttl) contains the dataset for testing the queries.

To contribute directly (by making comments) to the ontology on WebProtege use this link https://webprotege.stanford.edu/#projects/c4cdd618-b4c2-48e2-85c2-e08837e24669/edit/Classes?selection=Class(%3Chttps://purl.org/turbbo%23Barrier%3E)
