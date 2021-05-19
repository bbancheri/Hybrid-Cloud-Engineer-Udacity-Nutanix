# PROJECT SPECIFICATION #
## Private Cloud SaaS: Three-Tier Web Application ##
### Create Web App Blueprint Satisfying Business Requirements ###
CRITERIA | MEETS SPECIFICATIONS
------------- | -------------
Create a three-tier web application blueprint that satisfies the IaaS business requirements for self-service private cloud deployment and security scenarios.	| Submitted blueprint with : <li>Two SSH credentials.</li><li>Three VM service tiers.</li><li>One application profile.</li><li>VM and tasks follow security standards specified.

### Web Application Configuration and Scaling Web Tier ###
CRITERIA | MEETS SPECIFICATIONS
------------- | -------------
Enhance the blueprint to provide PaaS by configuring the web application and creating web-tier scaling. | Submitted a blueprint with three service VM configuration of: <li>Load balancer server software</li><li>Web tier server software</li><li>Database server software Submitted blueprint has:</li><li> | • a web application, configured to read and write to the database</li><li>post-deployment operation: a web-tier that scales and orchestrate updates to the load balancer configuration</li><li>web tier scaling actions that update the load balancer configuration

### Provide a Database Backup Action ###
CRITERIA | MEETS SPECIFICATIONS
------------- | -------------
Achieve a SaaS-like experience for developers with delegated, post- deployment operations to back up the database. | Submitted a blueprint with: a database backup custom action, which uses the root account with end-user provided password.

### Suggestions to Make Your Project Stand Out! ###
* Make the web tier scale-in and scale-out actions driven by a variable/macro provided by the user instead of hard coding it to a static value 1.
* Make the database backup action orchestrate the stop and (re)start of web tier or HAProxy services before and after backup for the proper order of operations.
* Make cloud-init leverage macros/variables instead of hard coding account names and public keys.
* Make the database password a mandatory field, use regular expressions to validate 8 or more characters.
