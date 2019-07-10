# Cybersecurity -REU- Project (By: Alejandro Zeno-Miranda and Daylyn Hoxie)

This README file will explain and guide on how to create the graph database we design in our REU internship at 
Montana State University in cybersecurity.

The main goal was to find flaws in a code or system by analyzing the impacts that some Attack Tactics using a 
variety of techniques can cause in different Common Weakness Enummeration (CWE).

The tool used for achieving this is Neo4j graph database management system developed by Neo4j, Inc. and Cypher query language. 
By creating a graph database we try to find a relationship between the CWEs and Attack Tactics through the impacts generated.

For the purposes of this text there is a zip file for every folder that contains everything needed for downloading and
recreating the proyect. Also, there is a zip file called "References" with all the links needed for following the explanation. 

We'll divide this proyect in two part since we worked in differents concepts to merge them later. 


Let's start:

# Common Weakness Enummeration (CWE) Part

As mentioned above, we are working with graph databases for this proyect. We used Neo4j and also Cypher query language. 
For those that are new in this DBMS and Cypher we put the link for a tutorial in the references zip file (recommended). This 
tutorial have a complete description on what is a graph database and a guide on how to set up the neo4j browser. 

The process of creating the database was very complex due to the humongous quantity of CWEs (hundreds surpassing the thousand) 
and Attack Tactics existing. Thanks to this factor, we decided to only work with the top 25 most dangerous software errors. In 
other words, the top 25 CWEs, but even with the top 25 CWEs, the work of creating the database was very extensive. After some 
research we found that Neo4j have the capability of loading csv files. Knowing this, we proceded to create excel files for 
later converting them to csv files and load them into neo4j. Note: This excel and csv files can be found in the "csv Files" and "Excel Files" folders zip files.

This csv files need to be copied at the "import" folder of the database for achieving the load process. This folder can be found by going to the neo4j browser -> pressing the "Manage" button of the desired database -> Open Folder -> Import. 


For loading the CWEs csv file we used the LOAD clause and the following command:

LOAD CSV WITH HEADERS FROM "file:///file_name" AS line
CREATE (:label {name that is going to have the attribute in your database: line.name of the attribute in the csv file})

Note: The name for the label that we choose is "CWE".

Example:

LOAD CSV WITH HEADERS FROM "file:///Cwes.csv" AS line
CREATE (:CWE {id: line.Id, name: line.Name, abstraction: line.Abstraction, structure: line.Structure, description: line.Description})


Sometimes after the extraction of the csv file for some reason it creates CWE nodes that does not exist in the main file. This nodes only contain an attribute, "Id". That "Id" is not real in the data provided with the csv file. The CWE nodes that need to exist will have the following attributes "Id", "Name", "Abstraction", "Structure", and "Description". After identifying the nonexisting nodes we delete them first before continuing with the creation of the relatioship of the CWEs.

For deleting the nonexisting CWE nodes we used the DELETE clause. Taking in consideration the quantity of those nodes it takes to much time to delete them one by one so we try to find a way to delete them all by one command. Since all the CWEs nodes (existing and nonexisting) have the "Id" attribute we can't use that attribute to deleting them because it will delete everything. For this reason, we choose to use another attribute that was NULL in the nonexisting CWE nodes.

For achieving this we used the following command:

MATCH (n:label)
WHERE n.name IS NULL
DETACH DELETE n

Having deleted the non-existing nodes, we continue to create relationships, but not first without knowing what these are. The Mitre webpage (which is our reference site) identify seven different relationships between the CWEs. Those are: "ParentOf", "ChildOf", "CanFollow", "CanAlsoBe", "CanPrecede", "PeerOf", and "MemberOf". The reason of knowing the relationships that the CWE have between them is to try finding where an attack can affect and how many CWEs can affect at once. For example, if an attack affect an Child CWE node, as a consequence, it will affect the Parent CWE node of this Child. For that reason, we choose neo4j as a tool for our project. We need a way to visualize this inmense connection among the CWEs and this was the most efficient way, a graph database.

Having said this, break down the relationships and creating them is our objective at this moment. As we already mentioned, we are working only with the top 25 CWEs. The Mitre site have a complete description of the top 25 CWEs including the relationships they have with other CWEs. For this proyect we only used the first generation (if we can call it like that) due to time limitations. We created another Excel file with all the information of the other CWEs that the top 25 CWEs are related to and then convert it to a csv file. As we did with the CWEs csv file, we put the Relationships csv file in the "import" file of the database we're already working with. 

Note: This excel and csv files can be found in the "csv Files" and "Excel Files" folders zip files.


For loading the relationships csv we used the follwing command: 

LOAD CSV WITH HEADERS FROM "file:///file_name" AS line
CREATE (:label {name that is going to have the attribute in your database: line.name of the attribute in the csv file})

Note: The name for the label that we choose is "Relationship".

Example:

LOAD CSV WITH HEADERS FROM "file:///CWEsRelationships.csv" AS line
CREATE (:Relationships {id: line.Id, name: line.Name, abstraction: line.Abstraction, structure: line.Structure, description: line.Description})


At this point we have created two labels, "CWE" and "Relationships" (in our case). After this we verify if there are nonexisting nodes created. If there are, delete them and procede to create the relationships. 

The command used for deleting the nonexisting nodes is:

MATCH (n:label)
WHERE n.name IS NULL
DETACH DELETE n


This is the most extensive part since to create the relationships they have to be created one by one due to the different existing relationships. Do not try to create all the different relationships in one command because it will create double relationships between the CWEs. If the relationship amoung the CWEs is the same, then it is possible to make them in one command. In other words, if a n CWE got two or more Child CWEs, then is possible to create both relationship in one command.

For creating the relationships we used the following command:

MATCH (a:label_1),(b:label_2)
WHERE a.id = "a.CWE" AND b.id = "b.CWE" 
CREATE (a)<-[r:type of relationship]-(b)
RETURN a,b

For example:

MATCH (a:CWE),(b:Relationship)
WHERE a.id = "CWE-79" AND b.id = "CWE-87" 
CREATE (a)<-[r:ParentOf]-(b)
RETURN a,b

If we want to add two CWE and relationship of the same type at the same time it will look like this:

MATCH (a:CWE),(b:Relationship),(c:Relationship)
WHERE a.id = "CWE-79" AND b.id = "CWE-87" AND c.id = "CWE-80" 
CREATE (a)<-[r1:ParentOf]-(b), (a)<-[r2:ParentOf]-(c)
RETURN a,b,c

Note: If there are more than two CWEs and relationships and want to put them in one command, just keep adding variables as much as needed. 


Now focusing in the "MemberOf" relationship. As we can see in the last commands the direction of the arrows in the las examples was like this (a)<-[]-(b) and not (a)-[]->(b). The reason for that is given to how the relationship is managed. For the relationships with the types of: "ParentOf", "ChildOf", "CanFollow", "CanAlsoBe", "CanPrecede", and "PeerOf" the direction it will be (a)<-[]-(b) but for the "MemberOf" relationship type it will be (a)-[]->(b). This is because the "MemberOf" relationship is a classification to group different CWEs that have certain similarities and the direction of the relationship indicates that certain CWE is part of that clasification.

This is how it will look a "MemberOf" command:

MATCH (a:CWE),(b:Relationship)
WHERE a.id = "CWE-79" AND b.id = "CWE-442" 
CREATE (a)-[r:MemberOf]->(b)
RETURN a,b


If the CWE is part of two clasification it will look like this:

MATCH (a:CWE),(b:Relationship),(c:Relationship)
WHERE a.id = "CWE-79" AND b.id = "CWE-442" AND c.id = "CWE-1019" 
CREATE (a)-[r1:MemberOf]->(b), (a)-[r2:MembertOf]->(c)
RETURN a,b,c

Note: If the CWE is part of more clasifications and want to put them in one command, just keep adding variables as much as needed. 


At this point of the guide we already have created the CWE database entirely. Now we are going to work with the backup, the exportation and importation of the database, and merging the CWE database with the Attack Tactics database. First, let's work with the backup part. For obtaining the backup folder we must find the "graph.db" folder in the database data located at the neo4j browser. For achieving this follow this path. neo4j browser -> press the "Manage" button of the desired database -> Open Folder -> data -> databases. Once there copy the "graph.db" folder in a safe place. With that folder it can recreate the same database if is copied in the "databases" folder of another empty database. 

Note: There is txt file with the instruction for exporting-importing the database. This file is located in the "Export-Import Database" zip file.


Now that we got the database backup we'll procede to the merging part. For merging the database we need the .cypher file that we obtain by entering a command in the database, but first is needed to download the APOC configuration and restarting the database (if is started). For downloading the APOC settings go to neo4j browser -> "Manage" button of the desired database -> Plugins -> Install APOC. 


Once the APOC is installed we go to the database settings and writte  "apoc.export.file.enabled=true" at the end of the code. 
This setting page can be found by going to neo4j browser -> "Manage" button of the desired database -> setting. Then go to the database and enter the following command: "CALL apoc.export.cypherAll('/usr/tmp/test1.cypher', {format:'plain'})". If the entered command give an error use this one: "CALL apoc.export.cypherAll('test1.cypher', {format:'plain'})". After this the .cypher file can be found at the "import" folder of the database. 


==========================================
STOP! From this part onwards it is required to have both databases created, the database of the CWEs and the database of Attack Tactics and Techniques. Skip this part and go to the "Attack Tactics and Techniques part".
=

Note: Make sure to create a new database for creating the merged one. Put the backup of the CWE database or the Attack Tactics and Techniques database in the new database and then load the .cypher file into that database. Also, copy the backup folder ("graph.db" folder) of that database since it will be a new one. 

For loading the .cypher file we don't use the LOAD clause. 

We are looking to pass the information of one database to the other one. First, open the .cypher file that we already obtained and it will open a text file. Second, copy all the text of the file. At last, paste it to the command line of the new database and run it. The time that it takes the process it will depend of the size of the database. When the process is completed it will include the database information to the one new database. As result, we'll have the merge database that we have been working for.


End of .cypher loading part.
==========================================





# Attack Tactics and Techniques





















==========================================
Futere Plans
==========================================
