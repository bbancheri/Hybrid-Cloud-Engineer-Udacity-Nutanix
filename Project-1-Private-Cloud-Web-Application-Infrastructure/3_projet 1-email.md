# Project submission #
## Email ##
Dear CTO, Systems Administrators, Network Operations teams,

We have decide to improve performance and availability of our production application stack in order to respond to company need. In this context, we have considering to enguage an hybrid could solution in order to meet the company current needs and expand to to cloud in the futur. An Hyperconverged infrastructure cluster was aquired in order to do a proof of concept.
After consultation of all takeholders, all requirements have been take in. 
Please found below in a first hand, a thorough summary of high level requirements, and in a second hand, a detailed performed task to meet each requirpements.

in a attachment, you will found a configuration document of the proof of concept of a 3tier application stack.

## High level requirements 
* our company have requirement to be could ready
	* we have detemining main business needs : performance and availability
	* we have enguage to plan an hybrid information system
* our hight level requirement are : 
	* 3-tier application (database server, application server, web server)
	* Separated production and development environments
	* Build-in data protection for production environment with snapshots , our SLA, the  maximum data lost after failure, is 1 hour
	* Ability to update on demand ressources to online virtual machines
	* Ability to increase VM workload by cloning
	* Create a disaster recovery strategy : ensure to to perform full restoration of a part or all the production environment \

## Please found now a descrption of performed tasks
* Update settings of the HCI Cloud Plaform
	* NTP server name : 
		* go to task menu > NTP server
		* add value : pool.ntp.org
	* DNS server : 
		* go to task menu > Name Servers
		* delete value 169.254.169.254, confirm
		* let value : 8.8.8.8
* Separate production and environment networks
	* go to task menu > Network Configuration
	* production network (managed network)
		* click on + Create Network
		* name : mng-prod-net
		* vlan : 0
		* subnet ip : 172.31.0.0
		* prefix length : 255.255.255.0
		* ip pool starting address : 172.31.0.210
		* ip pool ending address : 172.31.0.230
		* check override DHCP server (the value will be 172.31.0.254)
	* development network (unmanaged network)
		* name : unm-dev-net
		* vlan : 101
* Create a storage container to the HCI Cloud Platform and and populated by upload Windows and Linux (CentOS) virtual disk images
	* creation of Storage Container
		* go to menu > storage > + Storage Container
		* name of Storage Container : Images
		* click on save
	* Populate SC with image 
		* go to task menu > Image Configuration
		* click Upload Image
		* create Windows image 
			* name : Windows Server
			* Annotation : Windows Server
			* Image Type : disk
			* Storage Container : Images (previously created)
			* upload file from : c:\Images
			* file name : Windows.qcow2 + click open
			* click save
		* Linux image (CentOS)
			* name : CentOS Server
			* Annotation : CentOS Server
			* Image Type : disk
			* Storage Container : Images (previously created)
			* upload file from : c:\Images
			* file name : CentOS.qcow2 + click open
			* click save
* Deployment of three VM of 3-tier application 
	* procedure : 
		* go to menu > VM Table tab > + Create VM
	* roles
		* database server
			* vm name : db-prod-vm
			* vm description : Database server for Production
			* time zone : no changes
			* Compute Details : 
				* vCPU(s) : 1
				* Memory : 4 (GiB)
			* Disks
				* + Add New Disks
				* Type : DISK
				* Operation : Clone from Image Service
				* Image : Windows Server
				* let other fields by default
			* Networks Adapters (NIC)
				* + Add new NIC
				* Network Name : mng-prod-net
				* Click Add button
			* click Save button
			* Start VM : db-prod-vm 
			* Launch Console
			* Login to operating system : 
				* login : administrator
				* password : nutanix/4u
			* Change name server : db-prod-vm
			* ip address : dhcp assigned 
				* command line
				* ipconfig : 172.31.0.216
			* Install Nuntanix Guest Tools
				* go to menu > VM Table tab > select vm
				* click Manage Guest Tools
				* in Manage VM Gest Tools windows
				* click enable Nutanix Guest Tools
				* Mount Nutanix Guest Tools
				* Enable Application
					* check box Self Service Restore (SSR)
					* check box Volume Snapshot Service / Application Consistent Snapshots (VSS) 
				* Return in the Console server
				* Open Windows Explorer
				* Install NGT, let all options by default
			* Restart Windows Server
		* application server
			* name : app-prod-vm
			* description : Application server for Production
			* time zone : no changes
			* Compute Details : 
				* vCPU(s) : 1
				* Memory : 4 (GiB)
			* Disks
				* + Add New Disks
				* Type : DISK
				* Operation : Clone from Image Service
				* Image : CentOS Server
				* let other fields by default
			* Networks Adapters (NIC)
				* + Add new NIC
				* Network Name : mng-prod-net
				* Click Add button
			* click Save button
			* Start VM : db-prod-vm 
			* Launch Console
			* Login to operating system : 
				* login : root
				* password : nutanix/4u
			* Change name server : 
				* hostnamectl set-hostname app-prod-vm
			* ip address : dhcp assigned 
				* command line
				* ip addr : 172.31.0.230
			* Install Nuntanix Guest Tools
				* go to menu > VM Table tab > select vm
				* click Manage Guest Tools
				* in Manage VM Gest Tools windows
				* click enable Nutanix Guest Tools
				* Mount Nutanix Guest Tools
				* Enable Application
					* check box Self Service Restore (SSR)
					* check box Volume Snapshot Service / Application Consistent Snapshots (VSS) 
				* Return in the Console server
				* sudo mount /dev/sr0 /mnt
				* sudo /mnt/installer/linux/install_ngt.py
			* Restart CentOS Server
		* web presentation server
			* name : web-prod-vm
			* description : Web server for Production 
			* time zone : no changes
			* Compute Details : 
				* vCPU(s) : 1
				* Memory : 4 (GiB)
			* Disks
				* + Add New Disks
				* Type : DISK
				* Operation : Clone from Image Service
				* Image : CentOS Server
				* let other fields by default
			* Networks Adapters (NIC)
				* + Add new NIC
				* Network Name : mng-prod-net
				* Click Add button
			* click Save button
			* Start VM : db-prod-vm 
			* Launch Console
			* Login to operating system : 
				* login : root
				* password : nutanix/4u
			* Change name server : 
				* hostnamectl set-hostname web-prod-vm
			* ip address : dhcp assigned 
				* command line
				* ip addr : 172.31.0.210
			* Install Nuntanix Guest Tools
				* go to menu > VM Table tab > select vm
				* click Manage Guest Tools
				* in Manage VM Gest Tools windows
				* click enable Nutanix Guest Tools
				* Mount Nutanix Guest Tools
				* Enable Application
					* check box Self Service Restore (SSR)
					* check box Volume Snapshot Service / Application Consistent Snapshots (VSS) 
				* Return in the Console server
				* sudo mount /dev/sr0 /mnt
				* sudo /mnt/installer/linux/install_ngt.py
			* Restart CentOS Server
* Creation of virtal machines clones and change configuration
	* procedure
		* home > vm > select vm > clone
	* database server
		* number of clone : 1
		* vm name : db-dev-vm
		* vm description : Database server 
		* time zone : no changes
		* compute details : no changes
		* networks adapters
			* delete mng-prod-net / vlan 0
			* add umn-dev-net / vlan 101
		* start Windows database server
		* change name : db-dev-vm
		* Install Nuntanix Guest Tools
			* go to menu > VM Table tab > select vm
			* click Manage Guest Tools
			* in Manage VM Gest Tools windows
			* click enable Nutanix Guest Tools
			* Mount Nutanix Guest Tools
			* Enable Application
				* check box Self Service Restore (SSR)
				* check box Volume Snapshot Service / Application Consistent Snapshots (VSS) 
	* application server
		* number of clone : 1
		* vm name : app-dev-vm
		* vm description : Application server for development
		* time zone : no changes
		* compute details : no changes
		* networks adapters
			* delete mng-prod-net / vlan 0
			* add umn-dev-net / vlan 101
		* Install Nuntanix Guest Tools
			* go to menu > VM Table tab > select vm
			* click Manage Guest Tools
			* in Manage VM Gest Tools windows
			* click enable Nutanix Guest Tools
			* Mount Nutanix Guest Tools
			* Enable Application
				* check box Self Service Restore (SSR)
				* check box Volume Snapshot Service / Application Consistent Snapshots (VSS) 
	* web server
		* number of clone : 1
		* vm name : web-dev-vm
		* vm description : Web server for development
		* time zone : no changes
		* compute details : no changes
		* networks adapters
			* delete mng-prod-net / vlan 0
			* add umn-dev-net / vlan 101
		* Install Nuntanix Guest Tools
			* go to menu > VM Table tab > select vm
			* click Manage Guest Tools
			* in Manage VM Gest Tools windows
			* click enable Nutanix Guest Tools
			* Mount Nutanix Guest Tools
			* Enable Application
				* check box Self Service Restore (SSR)
				* check box Volume Snapshot Service / Application Consistent Snapshots (VSS) 
* Creation of a build-in dataprotection with protection group
	* prodecure
		home > data protection menu > table tab > + protection domain > Async DR
	* production protection domain
		* tab name : dp-prod
		* tab entities 
			* vm db-prod
				* check box db-prod-vm 
				* check box ''use application consistent snapshots''
				* check box ''auto protect related entities''
				* click protect selectd entities
			* vm app-prod
				* check box app-prod-vm 
				* check box ''use application consistent snapshots''
				* check box ''auto protect related entities''
				* click protect selectd entities
			* vm web-prod
				* check box web-prod-vm 
				* check box ''use application consistent snapshots''
				* check box ''auto protect related entities''
				* click protect selectd entities
		* click next
		* tab schedule > click New Schedule
			* configure your local schedule
				* repeat every 1 hour
				* start on (date) : let default value 
				* at (time) : let default value 
				* check box ''Create application consistent snapshot''
			* retention Policy
				* check box Local already checked
				* keep the last : 1 snapshots
			* click ''Create Schedule''
		* click button ''close''
		* on below tab, ensure tab local snapshot : statut is in progress 
* Update ressources of online virtual machines
	* tacke a snapshot of db-prod-vm
		* home > vm > table tab > 
		* select db-prod-vm
		* select take snapshot
		* snapshot name : before-resources-update
	* update ressources database server (refer to email)
		* go to home > vm > table tab
		* select vm : db-prod-vm
		* procedure update vCpu(s)
			* select update vm
			* go to label ''compute details''
			* in vCpu(s) : updated from 1 to 2
				* click on save
		* procedure update Memory
			* select update vm
			* go to label ''compute details''
				* Memory : updated from 4 to 6
				* click on save
		* verify changes
			* go in home > vm > table tab
			* select vm db-prod-vm
			* in column Core, verifing value is 2
			* in column Memory, verifing value is 6 GiB
* Perform test of disaster recovery strategy by deleting 1 or more production VM
	* go to home > data protection menu > table tab > + protection domain > ASYNC DR
	* select data protection domain db-prod
	* lower planel go to local snapshot tab
	* select last finished snapshot (made before db vm update)
	* select restore link
	* in ''restore snapshot'' window, select entitie vm dp-prod-vm
	* Restore Settings
		* check box ''create new entitie'' button
		* let default value for
			* VM Name Prefix field : Nutanix-Clone-
			* Volume Group Name Prefix field : Nutanix-Clone-
	* click the ''restore'' button
	* power on retored vm, ensure is start : ok
	* power off retored vm
