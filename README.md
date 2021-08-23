# TheALMVnet
Class project using Ansible
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Diagram](https://user-images.githubusercontent.com/83714303/130390164-2e5e9eb5-cbe1-4532-9a8f-e0fe632699ab.JPG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _YAML___ file may be used to install only certain pieces of it, such as Filebeat.
---
  - name: installing and launching filebeat
    hosts: webservers
    become: yes
    tasks:

    - name: download filebeat deb
      command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

    - name: install filebeat deb
      command: dpkg -i filebeat-7.6.1-amd64.deb

    - name: drop in filebeat.yml
      copy:
        src: /etc/ansible/files/filebeat-config.yml
        dest: /etc/filebeat/filebeat.yml

    - name: Enable and Configure System Module
      command: filebeat modules enable system

    - name: setup filebeat
      command: filebeat setup

    - name: start filebeat service
      command: service filebeat start

    - name: enable service filebeat on boot
      systemd:
        name: filebeat
        enabled: yes


This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly _avaliable____, in addition to restricting _access___ to the network.
- Load balancers plays an important role in security because of its off-loading function of a load balancer defends an organization against DDos attacks.
- A jumpbox is a secure computer that all admins first connect to before launching any administrative task.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _logs____ and system _metrics____.
- The filebeats monitors the logs files and collects log events and fowards them to Kibanna/Elasticsearch. 
- The metricbeats watch the systems performances.


| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
|JumpBoxPro| Gateway  | 10.0.0.4   | Linux (Unbuntu)  |
| Web-1    |Webserver | 10.0.0.5   | Linux (Unbuntu)  |
| Web-2    |Webserver | 10.0.0.7   | Linux (Unbuntu)  |
| ELK VM   |ELKserver | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the _Jumpbox____ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- HomeNetwork IP Address

Machines within the network can only be accessed by _home network IP____.
- JumpBox Provisoner IP(10.0.0.4) ELk VM (10.1.0.4)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses   |
|----------|---------------------|----------------------  |
| Jump Box |     Yes             |   Home Network IP      |
|DVWA Web-1|     No              |10.0.0.4 HomeNetworkIP  |
|DVWA Web-2|     NO              | 10.0.0.4 HomeNetworkIP |
|ELK VM    |     Yes             | 10.0.0.4 Home NetworkIP|
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it reduce the potential for error and increase productivity. It allows It admins to automate thier daily task which frees them to focus on efforts that help deliver more value to the business


The playbook implements the following tasks:
- Configure Webservers
- Install ELK Server
- Install Filebeats
- Install MetricBeat

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
![ELK Dockerps](https://user-images.githubusercontent.com/83714303/130390427-96544749-65a7-4dce-b683-76809b0e122d.JPG)
### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.7

We have installed the following Beats on these machines:
- Filebeats
- Metricsbeat

These Beats allow us to collect the following information from each machine:
- Filebeats fowards and centralizes log data. This info is sent to Kibanna/Elasticsearch providing a GUI to simply examine the information.

- Metricbeats provides information such as metrics from the operating system and services running on the server. Metricbeats collects this data and sends it to Kibanna.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _roles____ file to _/etc/ansible/files____.
- Update the _hosts____ file to include webservers IPS and ELKserver IPS
- Run the playbook, and navigate to _HTTP://<ElkVMIP>:5601.setup.php__ to check that the installation worked as expected.


- Copy the elk.playbook.yml file to /etc/ansible 
- Update the hosts file too include webservers IP and ELK VM IP
- Update each config file with ELKserver IP.
    -Kibanna - uncomment and replace localhost with local IP for ELK server
    -Elasticsearch - uncomment and replace local host with local IP for ELK Server


_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
