## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![image](https://user-images.githubusercontent.com/53101711/137891755-c992eb22-c851-4491-b645-ea322f25fc97.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the **yml** file may be used to install only certain pieces of it, such as Filebeat.

+ [Ansible Configuration File](https://github.com/spookyinfo/ProjectOne/blob/main/Ansible/Ansible.cfg)  
+ [DVWA Script](https://github.com/spookyinfo/ProjectOne/blob/main/Ansible/DVWA)  
+ [ELK Playbook](https://github.com/spookyinfo/ProjectOne/blob/main/Ansible/ELK)  
+ [Filebeat Playbook](https://github.com/spookyinfo/ProjectOne/blob/main/Ansible/Filebeat)  
+ [Metricbeat Playbook](https://github.com/spookyinfo/ProjectOne/blob/main/Ansible/Metricbeat)  
 

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network. This helps secure the network from attacks that seek to overload it. Jump Boxes offer the advantages of automation and further security by allowing for the centralized deployment of files.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

Filebeat monitors log files, collects events, and forwards these records to Elasticsearch or Logstash for indexing.

Metricbeat takes the metrics and statistics that it collects and exports them to the specified output, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

| **Name**             | **Function**   |  **IP Address**    | **Operating System** |
|----------------------|----------------|--------------------|----------------------|
| Jump-Box-Provisioner | Gateway        | 10.0.0.0           | Linux                |
| Web-1                | Web server     | 10.0.0.1           | Linux                |
| Web-2                | Web server     | 10.0.0.2           | Linux                |
| ELK                  | ELK Server     | 10.1.0.3           | Linux                |
| Load Balancer        | Load Balancer  | Static External IP | Linux                |
| Workstation          | Access Control | 97.120.30.181      | Windows              |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ELK Server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 97.120.30.181.

Machines within the network can only be accessed by Jump-Box-Provisiner.

A summary of the access policies in place can be found in the table below.

| **Name**             | **Publicly Accessible** |  **Allowed IP Addresses**            |
|----------------------|-------------------------|--------------------------------------|
| Jump-Box-Provisioner | No                      | Workstation Public IP on SSH 22      |
| Web-1                | No                      | 10.0.0.1                             |
| Web-2                | No                      | 10.0.0.2                             |
| ELK                  | No                      | Workstation Public IP using TCP 5601 |
| Load Balancer        | No                      | Workstation Public IP on HTTP 80     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because the installation of services can be optimized for repeated installation on different machines, and there is no need to write custom code that might contain errors.

The playbook implements the following tasks:

+ Installs Docker  
    ```name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present      # Use apt module
        ```
        
+ Increases Virtual Memory  
    ```name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144      # Use sysctl module
      ```
      
+ Downloads and installs a Docker ELK container  
     ```name: download and launch a docker elk container
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

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance:

![image](https://user-images.githubusercontent.com/53101711/137892555-0cc6255e-39d0-48d3-9b49-d0529de2974b.png)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
+ Web-1: 10.0.0.1
+ Web-2: 10.0.0.2

We have installed the following Beats on these machines:
+ Filebeat
+ Metricbeat

These Beats allow us to collect the following information from each machine:
+ Filebeat logs events
+ Metricbeat compiles metrics and system statistics

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the Ansible configuration file to /etc/ansible.
- Update the hosts file to include the ELK Server 
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

