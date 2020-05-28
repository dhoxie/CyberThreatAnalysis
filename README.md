# CyberThreatAnalysis
A Neo4j Graph Database [1] associating the ATT&CK Enterprise Matrix with the CWE/SANS Top 25 Most Common CWEs.

This threat analysis project connects all 12 tactics and 244 techniques from MITRE's ATT&CK Enterprise Matrix [2] with the CWE/SANS Top 25 Most Common CWEs [3].  ATT&CK tactics were associated with technical impacts using Izurieta and Prouty's mapping [4].

Connecting an ATT&CK tactic to a CWE through technical impacts allows for cause-and-effect analysis of an attack, giving insight into the goals of an attacker's exploit and allowing for improved prioritization of software weaknesses requiring attention.

Connections between the Top 25 CWEs and their related CWEs were also created using the ChildOf, CanAlsoBe, ParentOf, MemberOf, CanPrecede, and PeerOf relationships listed on MITRE's website.  These relationships can provide insight to similar weaknesses a user may want to explore.

The mapping of attack tactics to weaknesses within a graph database provides easy interpretation of these relationships from both developer and operational responder (SecDevOps) perspectives.

**Daylyn Hoxie and Alejandro Zeno-Miranda**

## Quickstart

1. Download, install, and start Neo4j Desktop.

2. Download DatabasesBackup.zip and unzip file.

3. Create a new graph in Neo4j Desktop in the project of your choice.

4. Within the new graph, select *Manage*, then *Open Folder*.  Within the folder that pops up, go to the "data" folder, then the "databases" folder.

5. Delete the graph.db file within the databases folder, then place the graph.db file from the downloaded and unzipped DatabasesBack folder.

6. Restart the graph.  Now the graph should contain all the information from the database.  You can access the data by opening the browser web interface at http://localhost:7474 or selecting *Open Browser* in Neo4j Desktop.



## References

[1] Neo4j Graph Platform. Neo4j Inc., 2019, https://neo4j.com.

[2] MITRE ATT&CK. The MITRE Corporation, 25 April, 2019, https://attack.mitre.org.

[3] CWE - Common Weakness Enumeration. The MITRE Corporation, 20 June, 2019, https://cwe.mitre.org.

[4] C. Izurieta, M. Prouty, "Leveraging SecDevOps to Tackle the Technical Debt Associated with Cybersecurity Attack Tactics", TechDebt'19. Montreal, Canada, May 2019. - related repository: https://github.com/maryeprouty/attack-analysis
