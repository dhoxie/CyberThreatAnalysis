# CybersecurityAssociationProject (By: Alejandro Zeno-Miranda and Daylyn Hoxie)

This README file will explain and guide on how to create the graph database we design in our REU internship at 
Montana State University in cybersecurity.

The main goal was to find flaws in a code or system by analyzing the impacts that some Attack Tactics using a 
variety of techniques can cause in different Common Weakness Enummeration (CWE).

The tool used for achieving this is Neo4j graph database management system developed by Neo4j, Inc. and Cypher query language. 
By creating a graph database we try to find a relationship between the CWEs and Attack Tactics through the impacts generated.

For the purposes of this text there is a folder called "Documentation" that contains everything needed for downloading and
recreating the proyect. Also, inside the "Documentation" folder there is another folder called "References" with all the links 
needed for following the explanation. 


Let's start:

As mentioned above, we are working with graph databases for this proyect. We used Neo4j and also Cypher query language. 
For those that are new in this DBMS and Cypher we put the link for a tutorial in the references document (recommended). This 
tutorial have a complete description on what is a graph database and a guide on how to set up the neo4j browser. 

The process of creating the database was very complex due to the humongous quantity of CWEs (hundreds surpassing the thousand) 
and Attack Tactics existing. Thanks to this factor, we decided to only work with the top 25 most dangerous software errors. In 
other words, the top 25 CWEs, but even with the top 25 CWEs, the work of creating the database was very extensive. After some 
research we found that Neo4j have the capability of loading csv files. Knowing this, we proceded to create excel files for 
later converting them to csv files and load them into neo4j. Note: This excel and csv files can be found in the "csv Files" and
"Excel Files" folders inside the "Documentation" folder.

For loading the csv file we used the LOAD clause and the following command:

LOAD CSV WITH HEADERS FROM "file:///file_name" AS line
CREATE (:label {name is going to have the attribute in your database: line.name of the attribute in the csv file})

Sometimes after the extract of the csv file for some reason it create nodes that does not exist in the main file. This nodes
only contain an "Id" that is not real in the data provided with the csv file. The nodes that need to exist will only have the 
following attributes "Id", "Name", "Abstraction", "Structure", and "Description".


