# PROJECT SPECIFICATION #
## Private Cloud Web Application Infrastructure ##
### Submission ###
CRITERIA| MEETS SPECIFICATIONS
------------- | -------------
Use the Configuration Documenter upon completion to gather the current cluster configuration. | Produced an Excel spreadsheet containing evidence of the proper configurations required.
Use content from the Configuration Documenter as a reference to send an email to the CTO summarizing action steps taken. | The contents of the email should include a summary of the stakeholders needs and an outline of the tasks performed for this project that meet these needs.&nbsp;The document produced with the Configuration Documenter utility will be sent as an attachment to the email.
### Configuration ###
CRITERIA | MEETS SPECIFICATIONS
------------- | -------------
Add an NTP server to the HCI Cloud Platform. | The cluster information for the NTP server uses the correct fully-qualified-domain-name in the provided email from the System Admin.
Add a Name Server to the HCI Cloud Platform. | The cluster information for the domain name server uses the correct IP address in the provided email from the System Admin.
Configure a managed virtual network using IPAM/DHCP in the HCI Cloud Platform. | The managed network is identified as the production network by name and has the correct Vlan, Subnet IP, Prefix Length, IP Pool starting and ending addresses, shown in the email provided by the Network Operations group.
Configure an unmanaged virtual network in the HCI Cloud Platform. | The unmanaged network is identified as the development network by name and has the correct Vlan and Prefix Length shown in the email provided by the Network Operations group.
Add a storage container to the HCI Cloud Platform and upload Windows and Linux (CentOS) virtual disk images to the new container. | The new container uses the name provided in the email from the Systems Administration group and is populated with the Windows and CentOS disk images.
Deploy Windows and Linux virtual machines in the HCI Cloud Platform. | The type and number of production virtual machines are identified by the resource specifications and designated naming convention outlined in the email from the Systems Administration group. These virtual machines must also be on the production network as designated in the email provided by the Network Operations group.
Create virtual machine clones and change configurations in the HCI Cloud Platform. | The type and number of development virtual machines are identified by the resource specifications and designated naming convention outlined in the email from the Systems Administration group. These virtual machines must also be on the development network as designated in the email provided by the Network Operations group.
Create data protection configurations to automate virtual machine snapshots. | The Data Protection domain(s) should be easily identified as the protection configurations for the production environment and with the recovery parameters provided in the email from the CTO. One protection domain or multiple protection domains are both acceptable.
Perform virtual machine restoration using the data protection snapshots. | The restored production virtual machine will be identified by the default restore naming convention as shown during the restore process in Prism.
