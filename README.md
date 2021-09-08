# Project1-ELK-stack
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Diagrams/Project1.png] (https://github.com/AnaPinigina/Project1-ELK-stack/blob/57f9ec43bd8cbeb2453babd5d3d529112eaee732/Diagrams/Project1.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

[Filebeat] (/AnaPinigina/Project1-ELK-stack/blob/57f9ec43bd8cbeb2453babd5d3d529112eaee732/Ansible/Filebeat.yml)

<block>
---
 - name: Installing and Launching Filebeat
  hosts: webservers
  become: yes
  tasks:

 - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.6.1-amd64.deb

  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat_config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: Enable and Configure System Module
    command: filebeat modules enable system

  - name: Setup filebeat
    command: filebeat setup

  - name: Start filebeat service
    command: service filebeat start

  - name: Enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes
</block>

This document contains the following details:

## Description of the Topology
* Access Policies
* ELK Configuration
  * Beats in Use
  * Machines Being Monitored
* How to Use the Ansible Build


Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D\\*mn Vulnerable Web Application.
Load balancing ensures that the application will be highly available, in addition to restricting in-bound access to the network.

What aspect of security do load balancers protect? 
**A load balancer intelligently distributes traffic from clients across multiple servers without the clients having to understand how many servers are in use or how they are configured. Because the load balancer sits between the clients and the servers it can enhance the user experience by providing additional security, performance, resilience and simplify scaling your website.

What is the advantage of a jump box?
**A jump box is a secure computer that all admins first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jump box and system network.

What does Filebeat watch for?
**Filebeat monitors the log files or locations being specified, collects log events, and forwards them either to Elasticsearch or Logstash for indexing

What does Metricbeat record?
**Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.
Note: Use the Markdown Table Generator to add/remove values from the table.



Name | Function | IP Address | Operating System
-----|----------|------------|-----------------
Jump Box | Gateway | 10.0.0.5 | Linux
Web-1 | Webserver | 10.0.0.6 | Linux
Web-2 | Webserver | 10.0.0.7 | Linux
ELK | Monitoring | 10.1.0.4 | Linux


## Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the jump box provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

* 5061 Kibana Port

Machines within the network can only be accessed by jump box provisioner.

Which machine did you allow to access your ELK VM? What was its IP address?
* My IP Address: 76.97.21.219

A summary of the access policies in place can be found in the table below.



Name | Publicly Accessible | Allowed IP Addresses
-----|---------------------|---------------------
Jump Box | Yes | 76.97.21.219
Web-1 | No | 10.1.0.4 
Web-2 | No| 10.1.0.4 
ELK | No | 10.1.0.4


## Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

TODO: What is the main advantage of automating configuration with Ansible?

The playbook implements the following tasks:

TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.
...
...

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
Note: The following image link needs to be updated. Replace docker_ps_output.png with the name of your screenshot image file.


Target Machines & Beats
This ELK server is configured to monitor the following machines:

TODO: List the IP addresses of the machines you are monitoring

We have installed the following Beats on these machines:

TODO: Specify which Beats you successfully installed

These Beats allow us to collect the following information from each machine:

TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., Winlogbeat collects Windows logs, which we use to track user logon events, etc.


Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:

Copy the _____ file to _____.
Update the _____ file to include...
Run the playbook, and navigate to ____ to check that the installation worked as expected.

TODO: Answer the following questions to fill in the blanks:

Which file is the playbook? Where do you copy it?
Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
_Which URL do you navigate to in order to check that the ELK server is running?
