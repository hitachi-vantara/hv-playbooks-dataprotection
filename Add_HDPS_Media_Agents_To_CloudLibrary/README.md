# This File explains how to run a YAML file to add another media agent and/or deduplication media agent  .

User Pre-requisites:
- A HDPS cloud library shoulde be already created.
- The user login password for the Command Center host must be used in Base64 format.
- The media agent should also be configured on the HDPS environment.
- Both the YAML fiels hosuld be placed under same folder
  
## 1. Enter the details in the file mediaAgent_vault.yml


   ### Required fields:
   - user_name: "Username of th HDPS command center"
   - user_pass: "Command Center Password in Base64 format"
   - HDPS_host: "Command Center Host IP"
   - HDPS_cloud_library: "name of the Cloud library were the media agents is to be added"
   - Add_Media_Agent_To_CloudLibrary: "Enter boolean value true/false if media agent is needed to be configured or not"  
   - Media_agent: "MediaAgent name" 
   - add_dedupeMediaAgent: Enter boolean value true/false if deduplication media agent is needed to be configured or not
   - Dedupe_Media_agent: "Name of the Deduplication-MediaAgent"
   - HDPS_validate_certs: false

	Example:
	user_name: "Username"
	user_pass: "Password_in_Base64"
	HDPS_host: "Host_IP"
	HDPS_cloud_library: "Existing Cloud library"
	Add_Media_Agent_To_CloudLibrary: true  
	Media_agent: "MediaAgent name" 
	add_dedupeMediaAgent: true
	Dedupe_Media_agent: "Deduplication-MediaAgent name"
	HDPS_validate_certs: false

   ### NOTE: Do not chnage tha variable in the mediaAgent_vault.yml file.
   
### 2. To trigger the YAML file to create the storage component:
   - Locate the YAML file: add_Media_Agents.yml
   - Run the following command:

     		ansible-playbook add_Media_Agents.yml

