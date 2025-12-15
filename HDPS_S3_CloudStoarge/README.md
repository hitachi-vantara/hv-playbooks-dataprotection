# This File explains how to run a YAML file to configure Storage components.

User Pre-requisites:
- An S3 bucket on VSP One Object should be created.
- The user login password for the Command Center host must be in Base64 format.
- S3 storage user credentials should already be saved in HDPS/Commvault Command Center for s3 endpoint.
  
## 1. Enter the details in the file hdps_vault.yml


   ### Required fields:
   - user_name: CommandCenter username to be entered.
   - user_pass: CommandCenter user password to be entered on base64 format"
   - HDPS_host: Enter HDPS/Commvault host IP
   - HDPS_cloud_library: "Enter desired HDPS cloud storage Name"
   - Media_agent: Provide Media agent name
   - Saved_Cred: "Enter the name of saved credentials used to reach S3 storage"
   - VSP1o_S3_Bucket_name: "S3_Bucket_name"
   - Enable_object_lock: Enter a boolean value for enable/disable storage lock
   - Retention_period: Enter the retention period for object lock.     #int value for retention period for storage lock
   - Dedupe_Media_agent: Enter dedupe media agent name if opting for it
   - Dedupe_Media_agent_path: Optionally, Provide path of dedupe media agent
   - Deduplication_enable: Enter a boolean value for enable/disable Deduplication option
   - s3_service_endpoint: Enter the S3 service endpoint of storage
   - HDPS_validate_certs: Enter boolean value for enable/disable cert validation

	Example:
	user_name: "CommandCenter_username"
	user_pass: "CommandCenter_Password_base64_format"
	HDPS_host: "HDPS_host_IP"

	HDPS_cloud_library: "HDPS_cloudStoarge_Name"
	Media_agent: "Media Agnet"
	Saved_Cred: "Saved_Credentials"
	VSP1o_S3_Bucket_name: "S3_Bucket_name"
	Enable_object_lock: false #boolean value for enable/disbale storage lock
	Retention_period: 22    #int value for retention period for storage lock
	Dedupe_Media_agent: "Dedupe Media agent"
	Dedupe_Media_agent_path: "Optional path value"
	Deduplication_enable: true #boolean value for enable/disbale Deduplication

	s3_service_endpoint: "s3.Region.vsp.domain.com(Region)"
	HDPS_validate_certs: false 
   
### 2. To trigger the YAML file to create the storage component:
   - Locate the YAML file: HDPS_S3_CloudStorage.yml
   - Run the following command:

     		ansible-playbook HDPS_S3_CloudStorage.yml
