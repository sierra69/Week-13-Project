# Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Azure Resource Group] (https://github.com/sierra69/Week-13-Project/blob/main/Week-13-Project/Diagrams/Azure%20Resource%20Group-Page-1.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the [install-elk.yml] file may be used to install only certain pieces of it, such as Filebeat.

  - Install-elk.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

```yml
---
- name: Installing and launching filebeat
  hosts: elk
  become: yes
  tasks:

    # Use command module
  - name: Download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

    # Use command module
  - name: Install filebeat deb
    command: dpkg -i filebeat-7.6.1-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml
 
    # Use command module
  - name: Enable and configure system module
    command: filebeat modules enable logstash

    # Use command module
  - name: Setup filebeat
    command: filebeat setup

    # Use command module
  - name: Start filebeat service
    command: sudo service filebeat start

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build
```

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly [available], in addition to restricting [traffic] to the network.
- _TODO: What aspect of security do load balancers protect? [defends an organization against distributed denial-of-service (DDoS)]
What is the advantage of a jump box?_Jump box prevents all Azure VM's to expose to the public. This means that this will be our entry point connecting via Remote Desktop Protocol (RDP) from our on-premise network. It also helps us to open only one port instead of several ports to connect different virtual machines present in the Azure cloud [availability]

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the [logs] and system [traffic].
- _TODO: What does Filebeat watch for?_[watches for log files/locations and collects log events]
- _TODO: What does Metricbeat record?_[ takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.]

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | Web S    | 10.0.0.5   | Linux            |
| Web-2    | Web S    | 10.0.0.6   | Linux            |
| Web-3    | Web S    | 10.0.0.8   | Linux            |
| Elk      | ElkVm    | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the [Jump-Box]machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- [76.124.*.*]

Machines within the network can only be accessed by [Jump-Box using ssh]
- _TODO: Which machine did you allow to access your ELK VM? [Jump-Box 10.4.0.4]
A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 76.124.*.*       |
| Web-1    | No                  | 10.0.0.5             |
| Web-2    | No                  | 10.0.0.6             |
| Web-3    | No                  | 10.0.0.8             |   
| ElkVm    | No                  | 10.1.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_You can put a command from multiple servers into a single playbook.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...[Install docker IO]
- ...[Install python pip]
- ...[Install docker]
- ...[systemctl -w vm.max_max_map_counts=26144]
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
[Web-1 10.0.0.5 ]
[Web-2 10.0.0.6]
[Web-3 10.0.0.8]
We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
[Filebeat and Metricbeat]

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
Fielbeat collect the changes done.
Metricbeat collect metric and statistics.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the [yml] file to [ansible].
- Update the [config]file to include...
- Run the playbook, and navigate to [Kibana] to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_[/etc/ansible/hosts]
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
[edit etc hosts fil to  webserves/elk server ip addresses]
- _Which URL do you navigate to in order to check that the ELK server is running? [http://137.135.*.*:5601/app/kibana#/home] note wild card added as a sucurity measure not to be hacked.

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
