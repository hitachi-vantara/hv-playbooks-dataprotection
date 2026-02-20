# Ansible Playbook to Automate Storage Configuration for Veeam Plug-in


## 1. Overview
This playbook automates the storage configuration process for Veeam Plug-in ([Storage Plug-in for Veeam Backup and Replication Implementation Guide v2.1](https://docs.hitachivantara.com/v/u/en-us/adapters/2.1.x/mk-92adptr154)).  
- Chapter 3: Pre-configuration settings details
  - Pre-configuration settings for Hitachi storage
    - Allocating backup resource pool (P16)
    - Configuring connectivity between Hitachi storage and various servers (P25)
    - Creating a resource group for Veeam® Backup & Replication™ and assigning storage resources for Hitachi Storage Plug-in to this resource group (P42)
    - Setting up a Veeam-only user account (P60)

By using this Playbook, storage settings can be automated, enabling users to utilize Veeam Backup & Replication, starting from 'Pre-configuration settings for Veeam® Backup & Replication™' (P68).

> **Note:**<br/>
> This Playbook does not support backup and recovery using remote copy configurations.<br/>
> Therefore, Configuring remote copy settings (P53) is not performed.


## 2. Prerequisites
- Ansible runtime environment is properly configured.
- Target storage is accessible from the Ansible runtime environment.
- [Hitachi Vantara VSP One Block Storage Modules for Red Hat® Ansible®](https://github.com/hitachi-vantara/vspone-block-ansible) is installed, and storage settings are described in `ansible_vault_storage_var.yml`.

> **Note:**
>  [Hitachi Vantara VSP One Block Storage Modules for Red Hat® Ansible®](https://github.com/hitachi-vantara/vspone-block-ansible) must be installed and configured because this playbook requires the module.
> - To use this playbook, you must fulfill the following requirements as described in the [Storage Plug-in for Veeam Backup and Replication Implementation Guide v2.1](https://docs.hitachivantara.com/v/u/en-us/adapters/2.1.x/mk-92adptr154).
>   - Chapter 2: Prerequisite requirements
>   - Chapter 3: Pre-configuration settings details
>     - Pre-configuration settings for Hitachi storage
>       - Installing program products (P14)

## 3. Storage configuration for Veeam Plug-in using Ansible Pipeline
This playbook consists of two separate playbooks, each designed for a different type of storage interface.  
Select and use the playbook depending on the target storage interface.
|#|Interface Type|Playbook|
|:---:|:---|:---|
|1|Fibre Channel (FC)|`storage_configurations_fc.yml`|
|2|FC/iSCSI Mixed|`storage_configurations_fc_and_iscsi.yml`|


### 3.1 Playbook for Fibre Channel (FC) Environment

#### 3.1.1 Description of Playbook for Fibre Channel (FC) Environment
This playbook separates the Veeam Plug-in configuration into the following process groups.
Each process group consists of multiple Ansible tasks.
|process groups|Process|Task name|
|:---:|:---|:---|
|Task1|Create DDP Pool|`Create a DDP Pool for Veeam Plug-in`<br/>`Debug ddp_pool_result`|
|Task2|Rename the DDP Pool name|`Update the DDP Pool Settings`<br/>`Debug rename_the_pool_name_result`|
|Task3|Change port security setting of the storage port by port id|`Change port security setting of the storage port by port id`<br/>`Debug port_security_result`<br/>`Change fabric mode and port connection settings of the storage port by port id`<br/>`Debug fabric_mode_and_port_connection_settings_result`|
|Task4|Create hostgroup|`Gather Unique Proxy Names`<br/>`Assign Host Group Numbers and Create Host Groups for Each Proxy`|
|Task5|Create a Resource Group with the following resources<br/>Resources : ldevs, ports, host groups, iSCSI targets|`Create a Resource Group with LDEVs and ports only`<br/>`Debug resource_group_ldev_ports_result`<br/>`Add Host Group`<br/>`Debug resource_group_hostgroup_result`|
|Task6|Create a User Group|`Create a User Group`<br/>`Debug user_group_result`|
|Task7|Create a User Account|`Create a User Account`<br/>`Debug user_result`|

> **Note:** Task1 and Task2
> - Use either Task 1 or Task 2, according to your DDP Pool creation policy.
>   - If creating a new DDP Pool, use Task1.
>   - If using an existing DDP Pool, use Task2 and comment out Task1.

> **Note:** Task4
> - Modify `proxies.yml` which is an external file required to execute the host group auto-creation.


#### 3.1.2 Playbook Parameter Settings for Fibre Channel (FC) Environments  
Below are the key parameters that require extra attention when setting up the playbook.
|process group|Task name|Parameter|Description|Setting example|
|:---:|:---|:---|:---|:---:|
|Task1|`Create a DDP Pool for Veeam Plug-in`|`pool_name`|**Parameter Requirement for pool_name**<br/>Please make sure to use the following format for the pool name:<br/>&emsp;&emsp;VBR_{any_value}<br/>Note: This naming rule is applicable only when creating a pool for use with the Plug-in.|"VBR_DDP_Pool_for_Backup"|
|Task2|`Update the DDP Pool Settings`|`pool_name`|**Parameter Requirement for pool_name**<br/>Please make sure to use the following format for the pool name:<br/>&emsp;&emsp;VBR_{any_value}<br/>Note: This naming rule is applicable only when creating a pool for use with the Plug-in.|"VBR_Rename_Pool"|
|Task5|`Create a Resource Group with LDEVs and ports only`|`name`|**Parameter Requirement for Resource Group name**<br/>Please use the following format for the Resource Group name:<br/>&emsp;&emsp;VBR_{any value}<br/>Note: This naming rule is applicable only when creating a Resource Group for use with the Plug-in.|"VBR_RG"|
|Task5|`Add Host Group`|`name`|**Parameter Requirement for Resource Group name**<br/>Please enter the resource group name configured in the "name" field of "Create a Resource Group with LDEVs and ports only" in the name field.|"VBR_RG"|
|Task6|`Create a User Group`|`role_names`|**Parameter Requirement for role_names**<br/>Please make sure to copy and paste the following value in the "role_names" field:<br/>&emsp;&emsp;"STORAGE_ADMIN_LOCAL_COPY", "STORAGE_ADMIN_PROVISION", "STORAGE_ADMIN_VIEW_ONLY"<br/>Note: Do not change the above values.|["STORAGE_ADMIN_LOCAL_COPY", "STORAGE_ADMIN_PROVISION", "STORAGE_ADMIN_VIEW_ONLY"]|

> **Note:**<br/>
> Comments are included for all required parameters within the playbook.<br/>
> Be sure to set all required parameters as directed in the comments.


#### 3.1.3 How to Run the Playbook for Fibre Channel (FC) Environments
Set all parameters in the Playbook and execute it using the following command.<br/>
`ansible-playbook storage_configurations_fc.yml`

> **Note:**<br/>
> If you are using Ansible Vault, you must specify the appropriate options.


### 3.2 Playbook for FC/iSCSI Mixed Environment


#### 3.2.1 Description of Playbook for FC/iSCSI Mixed Environment
This playbook separates the Veeam Plug-in configuration into the following process groups.
Each process group consists of multiple Ansible tasks.
|process groups|Process|Task name|
|:---:|:---|:---|
|Task1|Create DDP Pool|`Create a DDP Pool for Veeam Plug-in`<br/>`Debug ddp_pool_result`|
|Task2|Rename the DDP Pool name|`Update the DDP Pool Settings`<br/>`Debug rename_the_pool_name_result`|
|Task3|Change port security setting of FC storage port by port id|`Change port security setting of FC storage port by port id`<br/>`Debug port_security_result_fc`<br/>`Change fabric mode and port connection settings of FC storage port by port id`<br/>`Debug fabric_mode_and_port_connection_settings_result`|
|Task4|Change port security setting of iSCSI storage port by port id|`Change port security setting of iSCSI storage port by port id`<br/>`Debug port_security_result_iSCSI`|
|Task5|Create hostgroup|`Create hostgroup for VBR Plug-in`<br/>`Debug hostgroup_result`|
|Task6|Create iscsi target|`Create iscsi target for VBR Plug-in`<br/>`Debug iscsi_target_result`|
|Task7|Create a Resource Group with the following resources<br/>Resources : ldevs, ports, host groups, iscsi targets|`Create a Resource Group with LDEVs and ports only`<br/>`Debug resource_group_result`<br/>`Add Host Group`<br/>`Debug resource_group_hostgroup_result`<br/>`Add iSCSI target`<br/>`Debug resource_group_iscsi_target_result`|
|Task8|Create a User Group|`Create a User Group`<br/>`Debug user_group_result`|
|Task9|Create a User Account|`Create a User Account`<br/>`Debug user_result`|

> **Note:** Task1 and Task2
> - Use either Task 1 or Task 2, according to your DDP Pool creation policy.
>   - If creating a new DDP Pool, use Task1.
>   - If using an existing DDP Pool, use Task2 and comment out Task1.

> **Note:** Task5 and Task6
> - If multiple Veeam proxy servers are used, modify the playbook as follows:
>   1. Duplicate Task5 and Task6 for the number of Veeam proxy servers.
>   2. Modify parameters in Task5 and Task6 for each Veeam proxy server.


#### 3.2.2 Playbook parameter settings for FC/iSCSI Mixed Environment
Below are the key parameters that require extra attention when setting up the playbook.
|process group|Task name|Parameter|Description|Setting example|
|:---:|:---|:---|:---|:---:|
|Task1|`Create a DDP Pool for Veeam Plug-in`|`pool_name`|**Parameter Requirement for pool_name**<br/>Please make sure to use the following format for the pool name:<br/>&emsp;&emsp;VBR_{any_value}<br/>Note: This naming rule is applicable only when creating a pool for use with the Plug-in.|"VBR_DDP_Pool_for_Backup"|
|Task2|`Update the DDP Pool Settings`|`pool_name`|**Parameter Requirement for pool_name**<br/>Please make sure to use the following format for the pool name:<br/>&emsp;&emsp;VBR_{any_value}<br/>Note: This naming rule is applicable only when creating a pool for use with the Plug-in.|"VBR_Rename_DDP_Pool_for_Backup"|
|Task5|`Create hostgroup for VBR Plug-in`|`name`|**Parameter Requirement for hostgroup name**<br/>Please make sure to use the following format for the hostgroup name:<br/>&emsp;&emsp;VBR_{any_value}<br/>Note: This naming rule is applicable only when creating hostgroups for use with the Plug-in.|"VBR_HG_for_Proxy"|
|Task5|`Create hostgroup for VBR Plug-in`|`host_mode`|**Parameter Requirement for host_mode**<br/>Please make sure to copy and paste the following value in the "host_mode" field:<br/>&emsp;&emsp;VMWARE_EXTENSION<br/>Note: Do not change the above values.|"VMWARE_EXTENSION"|
|Task5|`Create hostgroup for VBR Plug-in`|`host_mode_options`|**Parameter Requirement for host_mode_options**<br/>Please make sure to copy and paste the following value in the "host_mode_options" field:<br/>&emsp;&emsp;[2, 22, 25, 40, 54, 63, 68, 91, 110]<br/>Note: Do not change the above values.|[2, 22, 25, 40, 54, 63, 68, 91, 110]|
|Task6|`Create iscsi target for VBR Plug-in`|`name`|**Parameter Requirement for iscsi target name**<br/>Please make sure to use the following format as the iscsi target name:<br/>&emsp;&emsp;VBR_{any_value}<br/>Note: This naming rule is applicable only when creating iscsi target for the Plug-in.|"VBR_iSCSI_for_Proxy"|
|Task6|`Create iscsi target for VBR Plug-in`|`host_mode`|**Parameter Requirement for host_mode**<br/>Please make sure to copy and paste the following value in the "host_mode" field:<br/>&emsp;&emsp;VMWARE_EXTENSION<br/>Note: Do not change the above values.|"VMWARE_EXTENSION"|
|Task6|`Create iscsi target for VBR Plug-in`|`host_mode_options`|**Parameter Requirement for host_mode_options**<br/>Please make sure to copy and paste the following value in the "host_mode_options" field:<br/>&emsp;&emsp;[2, 22, 25, 40, 54, 63, 68, 91, 110]<br/>Note: Do not change the above values.|[2, 22, 25, 40, 54, 63, 68, 91, 110]|
|Task7|`Create a Resource Group with LDEVs and ports only`|`name`|**Parameter Requirement for Resource Group name**<br/>Please use the following format for the Resource Group name:<br/>&emsp;&emsp;VBR_{any value}<br/>Note: This naming rule is applicable only when creating a Resource Group for use with the Plug-in.|"VBR_RG"|
|Task7|`Add Host Group`|`name`|**Parameter Requirement for Resource Group name**<br/>Please enter the resource group name configured in the "name" field of "Create a Resource Group with LDEVs and ports only" in the name field.|"VBR_RG"|
|Task7|`Add iSCSI target`|`name`|**Parameter Requirement for Resource Group name**<br/>Please enter the resource group name configured in the "name" field of "Create a Resource Group with LDEVs and ports only" in the name field.|"VBR_RG"|
|Task8|`Create a User Group`|`role_names`|**Parameter Requirement for role_names**<br/>Please make sure to copy and paste the following value in the "role_names" field:<br/>&emsp;&emsp;"STORAGE_ADMIN_LOCAL_COPY", "STORAGE_ADMIN_PROVISION", "STORAGE_ADMIN_VIEW_ONLY"<br/>Note: Do not change the above values.|["STORAGE_ADMIN_LOCAL_COPY", "STORAGE_ADMIN_PROVISION", "STORAGE_ADMIN_VIEW_ONLY"]|

> **Note:**<br/>
> Comments are included for all required parameters within the playbook.<br/>
> Be sure to set all required parameters as directed in the comments.


#### 3.2.3 How to Run the Playbook for FC/iSCSI Mixed Environment
Set all parameters in the Playbook and execute it using the following command.<br/>
`ansible-playbook storage_configurations_fc_and_iscsi.yml`

> **Note:**<br/>
> If you are using Ansible Vault, you must specify the appropriate options.


# **DISCLAIMER: **
All materials provided in this repository, including but not limited to Ansible Playbooks and Terraform Configurations, are made available as a courtesy. These materials are intended solely as examples, which may be utilized in whole or in part. Neither the contributors nor the users of this platform assert or are granted any ownership rights over the content shared herein. It is the sole responsibility of the user to evaluate the appropriateness and applicability of the materials for their specific use case.<br/>
Use of the material is at the sole risk of the user and the material is provided “AS IS,” without warranty, guarantees, or support of any kind, including, but not limited to, the implied warranties of merchantability, fitness for a particular purpose, and non-infringement. Unless specified in an applicable license, access to this material grants you no right or license, express or implied, statutorily or otherwise, under any patent, trade secret, copyright, or any other intellectual property right of Hitachi Vantara LLC (“HITACHI”). HITACHI reserves the right to change any material in this document, and any information and products on which this material is based, at any time, without notice. HITACHI shall have no responsibility or liability to any person or entity with respect to any damages, losses, or costs arising from the materials contained herein.
