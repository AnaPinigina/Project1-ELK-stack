# Project1-ELK-stack
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Diagrams/Project1.png](https://github.com/AnaPinigina/Project1-ELK-stack/blob/5eedb0fd1d8f784afb4c3668bd88e37d5e43f377/Diagrams/Project1.png)




These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

[Ansible/install_ELK.yml](https://github.com/AnaPinigina/Project1-ELK-stack/blob/1962d8a15ba35649884881db242f389632eac563/Ansible/install_ELK.yml)
```
---
# install_elk.yml
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: azadmin
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044
  ```




This document contains the following details:

**Description of the Topology
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

What is the main advantage of automating configuration with Ansible?
1. Agentless –There are no agents or software deployed on the clients/servers to work with Ansible. The connection can be done through the SSH or using the Python.
2. English Like Language – To use the Ansible, configure, and deploy the infrastructure is very simple and it is English like the language used called YAML.
3. Modular – The Ansible uses modules to automate, configure, deploy, and orchestrate the IT Infrastructure. There are around 750 + modules built-in Ansible.
4. Efficient – There are no servers, daemons, or databases required for Ansible to work.
5. Features – Ansible comes with a whole lot of features and can be used to manage the Operating systems, IT Infrastructure, the networks, the servers, and services in very less time.
6. Secure and consistent – Since the Ansible uses SSH and Python it is very secure and the operations are flawless.
7. Reliable – The Ansible Playbook can be used to write programs or the modules and can be used to manage the IT without any downside.
8. Performance- The Ansible’s performance is excellent and has very little latency.
9. Low Overhead – As it is agentless and does not require any servers, daemons, or databases it can provide a lot of space in the systems and has low overhead in terms of deployment.
10. Simple – It is very simple to use and is supported by YAML 

The playbook implements the following tasks:

TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.
* Install docker.io
* Install pip3
* Install Docker python module
* Increase virtual memory
* Download and launch a docker

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.


![sebp-elk (2)](https://user-images.githubusercontent.com/47455752/132564827-0783f376-4384-4457-b074-513398c6ecea.png)



## Target Machines & Beats
This ELK server is configured to monitor the following machines: private IPs of Web-1 and Web-2

Name | IP Address
-----|-----------
Web-1 | 10.0.0.6
Web-2 | 10.0.0.7


We have installed the following Beats on these machines:

* Microbeats

These Beats allow us to collect the following information from each machine:

* Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
* Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

## Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

* Copy the playbook file to Ansible control node.

![roles_yml](https://user-images.githubusercontent.com/47455752/132564235-148e9d12-9f35-4356-92e5-5680ad6bb997.png)


* Update the hosts file to include webservers and elk.
* Run the playbook, and navigate to [Kibana](http://20.105.192.121:5601/app/kibana#/home) to check that the installation worked as expected.
http://[Host IP]/app/kibana#/home
```
 $ ansible-playbook install-elk.yml elk
 $ ansible-playbook filebeat-playbook.yml webservers
 $ ansible-playbook metricbeat-playbook.yml webservers
 ```
 
![metricbeat_kibana](https://user-images.githubusercontent.com/47455752/132563933-cf4887ba-6e36-414a-8e70-005abdbe7740.png)
![filebeat_kibana](https://user-images.githubusercontent.com/47455752/132563938-e6a90f5f-6afd-4183-bc57-23da9ad88b2b.png)




Answer the following questions to fill in the blanks:

Which file is the playbook? Where do you copy it?
.YML files in roles directory : /etc/ansible/roles

![roles_yml](https://user-images.githubusercontent.com/47455752/132574699-ca90a673-ccbd-407a-abf4-2a6af7963c0c.png)


[Ansible/Filebeat.yml](https://github.com/AnaPinigina/Project1-ELK-stack/blob/ffe98578f7bda21c538a95d653328865136be7b3/Ansible/Filebeat.yml)


[Ansible/metricbeat.yml](https://github.com/AnaPinigina/Project1-ELK-stack/blob/378367788c62786b2050dc3bc34993f59ddc5bde/Ansible/metricbeat.yml)


How do I specify which machine to install the ELK server on versus which to install Filebeat on?

![ELK_hosts](https://user-images.githubusercontent.com/47455752/132574590-c129d47f-c2a3-4e90-9050-3fa290eb0e49.png)

![filebeat_webservers](https://user-images.githubusercontent.com/47455752/132574609-1df1efbd-15a3-43f7-bb26-e2ba5c74d06b.png)


Which URL do you navigate to in order to check that the ELK server is running?
* http://20.105.192.121:5601/app/kibana#/home
*![kibanaebana (2)](https://user-images.githubusercontent.com/47455752/132579783-309b0564-5ed9-48a0-83c5-d17744edf1da.png)

