

# This File explains how to run a YAML file to create a Veeam storage repository.

## User Pre-requisites:
- The Veeam version must be v12 or higher. All three files should be located in the same folder.
- The WinRM service must be enabled on the Windows machine and configured to listen on port 5985.
- The Ansible Windows module must be installed at system where the script is going to run.
- The AWS Cloud user credentials should be configured in Veeam BnR, as its access key will be used in the variable file to detect which account to be used.
      
      https://galaxy.ansible.com/ui/repo/published/ansible/windows/
      
## 1. Enter the details in the file to execute:
   
   ### Required fields for hosts.ini file:
   - [windows_host]: "provide your Veeam server IP under it"
   - ansible_user: "Provide server user name"
   - ansible_password: "Provide Server Password"
   - ansible_connection=winrm (mention Windows service)
   - ansible_winrm_server_cert_validation=ignore (to skip the cert validation)

Example:

    [windows_host]
    182.26.95.11

    [windows_host:vars]
    ansible_user=Admin
    ansible_password=Hello123
    ansible_connection=winrm
    ansible_winrm_transport=basic
    ansible_port=5985
    ansible_winrm_server_cert_validation=ignore

   ### Details to be entered to execute the bucket configuration :
   - Region: Mention the region of the VSP One Object
   - Service_Point: Mention the S3 service endpoint of VSP One Object
   - Access_Key: provide the access key of the user/account as the veeam may have multiple users.
   - Storage_bucket: Mention the S3 bucket name, which is already created on VSP One Object
   - Bucket_folder: Mention the folder name that is to be created inside the bucket during Veeam storage creation.
   - Veeam_Stoarge_name: Veeam Stoarge repository name
   - Veeam_Storage_Description: write Description of the Veeam Storage repository.
   - EnableImmutability: Give a value in bool to enable retention or not; you have to enable it if the S3 bucket on VSP One Object has Object Lock/immutability enabled.
   - ImmutabilityPeriod: Enter the number of days for the retention period to be enabled when immutability is enabled on the VSP One Object 
   - EnableBucketAutoCreate: false
   - Veeam_MaxConcurrentTasks: Here you can mention an integer number that determines the number of concurrent tasks to be run during backup to this Veeam storage repository.
   - Connection_type: "Gateway"  #Mention connection type "Gateway" or "Direct" as per requirement
   - LimitStoargeConsumtion: Enter the bool value if u want to restrict storage consumption on the S3 storage bucket.
   - StorageSizeLimit: 10240   # This value is in GB, and the minimum limit is 10 TB, which equals 10240 GB.
   - Gateway_servers: Enter the gateway IP or name to be used as the gateway. At least one gateway IP should be mentioned, even if the connection type is "Direct"

Example:

      Region: "cluster01"
      Service_Point: "s3.us-east-1.vsp1o.domain.com"
	  Access_Key: "AKIAJlUvsoWiDxRINikQ"
      Storage_bucket: "s3bucket"
      Bucket_folder: "TestFolder"
      Veeam_Stoarge_name: "Ansible Bucket"
      Veeam_Storage_Description: "Testing Ansible"
      EnableImmutability: true
      ImmutabilityPeriod: 1
      EnableBucketAutoCreate: false
      Veeam_MaxConcurrentTasks: 2
      Connection_type: "Gateway"   #Mention connection type "Gateway" or "Direct" as per requirement
      LimitStoargeConsumtion: false
      StorageSizeLimit: 10240   # This value is in GB, and the minimum limit is 10 TB, which equals 10240 GB.
      Gateway_servers:
        - WIN-R9TKQ483GKU
	

### 2. To trigger the YAML file to create the storage component:

   - Locate the YAML file: ps_veeamBucket.yml
   - Run the following command:

    ansible-playbook -i hosts.ini ps_veeamBucket.yml

     		
