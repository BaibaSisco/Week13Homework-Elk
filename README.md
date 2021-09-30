## Automated ELK Stack Deployment

The files in this **repository** were used to configure the network depicted below.

![image](https://user-images.githubusercontent.com/91572862/135367918-e34d6d22-c2c9-4f3c-bc20-1935c5193371.png)





These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the *playbook (yml)* file may be used to install only certain pieces of it, such as Filebeat.

 
 

  [install_elk.yml](https://github.com/BaibaSisco/Week13Homework-Elk/blob/main/Ansible/install-elk.yml)
  
This document contains the following details:
* Decription of Topology
* Access Policies
* ELK Configuration - (Beats in Use and Machines Being Monitored )
* How to use the Ansible build 

 

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly *available*, in addition to restricting *access* to the network.

* Load balancers are responsible to ensure availability to the web servers.Because the load balancer is between the clients and the servers it can enhance the user experience by providing additional security, performance and simplify scaling your website. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.



* Advantage of the Jump Box is having one point for administrative tasks, and it is secure.
Administrators are required to access the Jump Box first before accessing the other servers.



* Filebeat watches log files and locations to collect log events.

* Metricbeat records metrics and statistical data from the operating system and from services running on the server.


The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.



| Name     | Function   | IP Address | Operating System |
|----------|----------  |------------|------------------|
| Jump Box | Gateway    | 10.0.0.4   | Linux            |
| Web-1    | Webserver  | 10.0.0.5   | Linux            |
| Web-2    | Webserver  | 10.0.0.6   | Linux            |
| ELK-VM   | Monitoring | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the *Jump Box* machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

`5601 Kibana Port` 

Machines within the network can only be accessed by Jump Box.

* The ELK VM is only accessible via SSH from the Jump Box and through web access from `Host IP address`. 

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | Host IP Address      |
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             |
| ELK-VM   | No                  | 10.0.0.4             |           


### Elk Configuration

[Pentest.yml](https://github.com/BaibaSisco/Week13Homework-Elk/blob/main/Ansible/Pentest.yml)

      


Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
 *What is the main advantage of automating configuration with Ansible?*

* Deployment of multiple servers easily without touching each server is the main advantage of authomating configuration with Ansible.
* Efficient- because you don't need to install any extra software. 
* Powerful- Ansible lets you model even highly complex IT workflows.

The playbook implements the following tasks:
*In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.*

 * Install Docker.io
 * Increase VM memory
 * Install docker Python model
 * Dowload and launch ELK Docker container 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image](https://user-images.githubusercontent.com/91572862/135363520-37872270-de5c-472b-9cf6-3f36f7b81ad4.png)



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
*List the IP addresses of the machines you are monitoring*

| Web-1 | 10.0.0.5 |
|-------|----------|
| Web-2 | 10.0.0.6 |



We have installed the following Beats on these machines:
*Specify which Beats you successfully installed*

`Filebeat`
`Metricbeat`

These Beats allow us to collect the following information from each machine:
*In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.*

* Filebeat monitors for log files and locations and collects log activities.
  (Filebeat: Lightweight Log Analysis & Elasticsearch)

* Metricbeat records metrics and statistical data from the operating system and from services running on the server.
  (Metricbeat: Lightweight Shipper for Metrics)
  
  ![metricbeat-playbook.yml] 
  ---
  - name: Install metric beat
    hosts: webservers
    become: true
    tasks:
    # Use command module
  - name: Download metricbeat
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb

    # Use command module
  - name: install metricbeat
    command: dpkg -i metricbeat-7.4.0-amd64.deb

    # Use copy module
  - name: drop in metricbeat config
    copy:
      src: /etc/ansible/files/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml

    # Use command module
  - name: enable and configure docker module for metric beat
    command: metricbeat modules enable docker

    # Use command module
  - name: setup metric beat
    command: metricbeat setup

    # Use command module
  - name: start metric beat
    command: service metricbeat start


### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ssh-keygen file to ssh public key.
- Update the security rule file to include the IP address.
- Run the playbook, and navigate to ELK VM Public IP:5601/app/kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

