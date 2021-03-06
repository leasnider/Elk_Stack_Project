## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.
![Elk Stack Project Diagram](https://user-images.githubusercontent.com/91147087/155912807-96859e1c-4ead-43f7-a56a-2c7acf6e83c2.png)



These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  https://github.com/leasnider/Elk_Stack_Project/blob/c22c7bb924aa6d0890f328690f057bcd72c60080/Ansible/elk.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting traffic to the network.
- Having a Load Balancer adds additional layers of security from potential threats such as a DDoS attack and authenticates user access. The additional added layer also serves the purose of a "bridge" between the two trusted networks and is used as a single entryway to a server group.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat: Monitors log files or other specified locations and collects log events, then forwards them to Elasticsearch or Logstash for indexing.
- Metricbeat: Used to collect data such as CPU, load in a dock environment and memory.

The configuration details of each machine may be found below.


| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| JumpBoxProvisioner | Gateway  | 23.99.194.126/10.0.0.5   | Linux            |
| Web-1     |   Webserver       |   10.0.0.9         |   Linux               |
| Web-2    |   Webserver       |    10.0.0.10        |      Linux            |
| ELK-SERVER    |      Elk Stack    |     10.1.0.4       |        Linux          |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBoxProvisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 23.99.194.126

Machines within the network can only be accessed by JumpBoxProvisioner.
- JumpBoxProvisioner has access to the ELK-SERVER
- 10.0.0.5

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| JumpBoxProvisioner| Yes              | 23.99.194.126        |
|   Web-1       |    No                 |      10.1.0.4                |
|   Web-2      |      No               |       10.0.0.10               |
|   ELK-SERVER       |      No               |      10.0.0.4                |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows complex routine tasks to be done and automated therefore saving the person time as well as decreasing the risk of error with repeated tasks.

The playbook implements the following tasks:
- Install Docker.io
- Install Python3-pip
- Install Docker module
- Increase virtual memory
- Download and launch a docker elf container
- Enable service docker on boot


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
![Elk-Server](https://user-images.githubusercontent.com/91147087/156050002-ef3cba6b-ab7b-4fee-9e37-a13e6ba98c45.png)



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 (10.0.0.9)
- Web-2 (10.0.0.10)

We have installed the following Beats on these machines:
- Filebeats
- Metricbeats

These Beats allow us to collect the following information from each machine:
- Logbeats monitors specified log files or locations that are supposed to collect log events to be fowarded for indexing.
- Metricbeats monitors a servers metrics and statistics like inbound and outbound traffic that output such as Logstash.


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Nano into etc/ansible/hosts so the hosts file can be edited for the playbook to run on a specific machine
- Specify the IP addresses of the Web VM's under "#[webservers]". Remove the "#" comment.
  
  - '10.0.0.9 ansible_python_interpreter=/usr/bin/python3'
  
  - '10.0.0.10 ansible_python_interpreter=/usr/bin/python3'

- Create a new "(Elk)" group under the newly added web VM's and add the private IP show below the ELK-SERVER
  - '10.1.0.4 ansible_python_interpreter=/usr/bin/python3
- Save and exit

Name: filebeat-config.yml located inside /etc/ansible

- Run 'curl
  https://github.com/leasnider/Elk_Stack_Project/blob/4abbdc81e937e3b596dfe08066ffaf931a23fb75/Linux/filebeat-config.yml
- Use nano to update the filebeat-config.yml file so that the ELK-SERVER's IP is at line 1106 and 1806
  - Line 1105 'hosts: ["10.1.0.6:9200"]'
  - Line 1806 'host "10.1.0.6:5601"'

- Save and exit

Name: filebeat-playbook.yml, located in /etc/ansible/roles

- Create the playbook by running 'nano metricbeat-playbook.yml' then write the folling shown in the image below.

  https://github.com/leasnider/Elk_Stack_Project/blob/b003764cf07de50b98dbf4c071ff0af466f8c0fb/Ansible/metricbeat-playbook.yml
  
 - Save and exit

- In the same directory, run the playbook with the following command; 'ansible-playbook metricbeat-playbook.yml'
- Navigate to http://20.127.31.26:5601 to make sure the playbook ran correctly and is working.
![kabanaaaaaa](https://user-images.githubusercontent.com/91147087/156064357-50ccbebd-e92e-40ef-bb31-52c461615502.png)


