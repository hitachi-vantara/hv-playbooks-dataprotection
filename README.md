# Hitachi Data-Protection YAML playbooks
This repository is intended for internal use by the SEA DMS Object Team at Hitachi Vantara. It serves as a centralized location to store development scripts and related work created by the team. Note: This is not an official repository and is not intended for public presentation or formal release.
## Hardware requirements
VSP One Object 3.1
## Software requirements
Red Hat Ansible Core - 2.18, 2.19, or latest

Python - 3.7 or higher with the package "boto3-1.40.38" and "botocore-1.40.38" in the Python library, so that S3 connections can happen

## Supported operating systems
Oracle Enterprise Linux 8.9 or higher

Red Hat Enterprise Linux 8.9 or higher

## Recommended Host configuration
CPU/vCPU - 2

Memory - 4 GB

HardDisk - 30 GB

## Installation
Before using this collection, you need to install "ansible.windows" modules with the Ansible Galaxy command-line tool:

    ansible-galaxy collection install ansible.windows

Now clone the repository and use the Ansible YAML files to execute.

## **DISCLAIMER: **
All materials provided in this repository, including but not limited to Ansible Playbooks and Terraform Configurations, are made available as a courtesy. These materials are intended solely as examples, which may be utilized in whole or in part. Neither the contributors nor the users of this platform assert or are granted any ownership rights over the content shared herein. It is the sole responsibility of the user to evaluate the appropriateness and applicability of the materials for their specific use case. Use of the material is at the sole risk of the user, and the material is provided “AS IS,” without warranty, guarantees, or support of any kind, including, but not limited to, the implied warranties of merchantability, fitness for a particular purpose, and non-infringement. Unless specified in an applicable license, access to this material grants you no right or license, express or implied, statutorily or otherwise, under any patent, trade secret, copyright, or any other intellectual property right of Hitachi Vantara LLC (“HITACHI”). HITACHI reserves the right to change any material in this document, and any information and products on which this material is based, at any time, without notice. HITACHI shall have no responsibility or liability to any person or entity with respect to any damages, losses, or costs arising from the materials contained herein.

