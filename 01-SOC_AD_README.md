# SOC with Active directory project

Tools used in this project:
* Splunk for SIEM, Slack for case management and Suffle for SOAR capabilities. 
* Small active directory environment with user to perform authentication
* 3 VM-s are created in the cloud using Vultr

## Showcase of data flow

![SOC-Automation_project_dataflow](https://github.com/SivanS-iT/SOC_projects/blob/main/Images/01-SOC-AD/01-SOC_AD_Dataflow.png?raw=true)


## Description

The goal of this project is to create a real-world simulation of a SOC analyst’s role. In this scenario, an attacker machine successfully logs into a test system, triggering a playbook that requires the SOC analyst to assess whether the activity represents a legitimate threat. 
Once the primary objective is achieved, `additional automation will be integrated` to enhance the workflow.

## Setup

Here are the steps of project:
1. Setting up 2 windows servers and 1 Ubuntu server in `Vultr`
2. Configuring and protecting them with firewall so I will only be able to access them
3. Configuring main Win Server (Sambol.local) Acive directory and promoting it to domain controller
4. Joining Test machine to `Sambol` domain



