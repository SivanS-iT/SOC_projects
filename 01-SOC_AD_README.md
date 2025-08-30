# SOC with Active directory

<h4>
  <p><strong>Table of Contents</strong></p>
  <ol>
    <li><a href="#description">Description</a></li>
    <li><a href="#showcase-of-data-flow">Showcase of data flow</a></li>
    <li><a href="#setup-and-overview">Setup and overview</a></li>
    <li><a href="#project-images">Project images</a></li>
    <li><a href="#errors-along-the-way">Errors along the way</a></li>
  </ol>
</h3>


## Description

Tools used in this project:
* `Splunk` for SIEM, Slack for case management and `Suffle` for SOAR capabilities. 
* Small active directory environment with user to perform authentication
* 3 VM-s are created in the cloud using Vultr

The goal of this project is to create a real-world simulation of a SOC analyst’s role. In this scenario, an "attacker machine" successfully logs into a test system, triggering a playbook that requires the SOC analyst to assess whether the activity represents a legitimate threat. 
Once the primary objective is achieved, `additional automation will be integrated` to enhance the workflow.

## Showcase of data flow

![SOC-Automation_project_dataflow](https://github.com/SivanS-iT/SOC_projects/blob/main/Images/01-SOC-AD/01-SOC_AD_Dataflow.png?raw=true)


## Setup and overview

Here are the steps of project:
1. Set up two Windows servers and one Ubuntu server in `Vultr`.
2. Configure and protect them with a firewall so only I have access to them.
3. Configure the Active Directory of the main Windows server (Sambol.local) and promote it to a domain controller.
4. Join Test machine to `Sambol` domain
5. Install Splunk on the Ubuntu server
    * Installation is done by downloading and running the Splunk .deb file.
    * Then run the command /opt/splunk/bin# ./splunk start to accept user rights.
    * This starts the remaining configuration.
    * Port 8000 must also be enabled in the Ubuntu firewall with the following command:
        ```
        ufw allow 8000
        ```
6. Install the Universal Forwarder on both Windows servers (domain controller and test computer) and configure it
7. Test whether the Windows servers send events through the Universal Forwarder to the Ubuntu server where Splunk is installed
8. Query event data in the Splunk dashboard such as:
    ```
    index="sambol-ad" EventCode=4624 Source_Network_Address=* Source_Network_Address!="-"
    ```
9. Test various different search filters
10. Create custom alert for devices (different public IP address of my PC) that try to RDP into server. `Goal is to create alert using my PC but with VPN (using different public IP) and that way trigger playbook using Slack`.

    Alert search query created in Splunk:
    ```
    index="sambol-ad" EventCode=4624 Source_Network_Address=* Source_Network_Address!="-" Source_Network_Address!="86.*"
    |stats count by _time, ComputerName,Source_Network_Address,user,Logon_Type
    ```
    * This search query looks for `Source_Network_Address` that is different from my public IP address that starts with 86.x.x.x and displays it in a user frendly way


## Project images

`1. Screenshot of my dashboard in splunk executing login filter query`

![SOC-Automation_project_Query](https://github.com/SivanS-iT/SOC_projects/blob/main/Images/01-SOC-AD/01-AD-SplunkQuery.png?raw=true)


## Errors along the way

* Currently I have a problem that my test windows server is not sending or doesn't have connection to Ubuntu server where the Splunk is. So I am not getting any telemetry from windows logs
    ```
    FIX: with a lot if failed tries, deleting/installing universal forwarder as well as configuring outputs.config file and testing connection. Problem was that I haven't login to splunk as my universal forwarder required my credentials.
    ```
