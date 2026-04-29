**Overview:**
- BloodHound ingests AD data (users, groups, computers, GPOs, ACLs, trusts, sessions, local admin rights, RDP/WinRM access) and maps attack paths using graph theory.
- Two components: a collector (BloodHound.py for Linux, SharpHound for Windows) that outputs JSON files, and the BloodHound GUI backed by a Neo4j graph database.
- Collected data is uploaded to the GUI and queried via built-in path-finding queries or custom Cypher queries to identify privilege escalation paths to Domain Admin.
- Even indirect or multi-hop paths (user → group → ACL → host → DA) are surfaced automatically, making it the fastest way to identify non-obvious escalation routes.
---
## Collection

```
sudo bloodhound-python -u <user> -p <password> -ns <dc-ip> -d <domain> -c all
```

Run the BloodHound ingestor from a Linux attack host with valid domain credentials. `-ns` sets the nameserver to the domain controller, `-d` specifies the target domain, `-c all` runs all collection methods. Outputs JSON files in the current directory named by date and object type.

```
zip -r <out-file>.zip *.json
```

Compress all JSON output files into a single archive for upload to the BloodHound GUI.

---
## GUI Setup

```
sudo neo4j start
```

Start the Neo4j database service before launching BloodHound. Required for the GUI to function.

```
bloodhound
```

Launch the BloodHound GUI. Log in with Neo4j credentials, then upload the zip file via the `Upload Data` button. Use the `Analysis` tab to run built-in path-finding queries or custom Cypher queries.