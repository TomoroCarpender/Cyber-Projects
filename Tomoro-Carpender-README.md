# Cyber-Projects

Cloud Network

This is a repository of Linux Scripts and Ansible Scripts which I created in my Cybersecurity Class.

The scripts were created for the most part to configure the cloud servers and the docker container which I setup.

For this setup I had 4 servers running vulnerable DVWA containers along with a Jump-Box and a server which was running an ELK Stack container.



The files in this repository were used to configure the network depicted below.

![Tomoro Carpender (1)](https://user-images.githubusercontent.com/86326713/123363587-14db6080-d530-11eb-853d-c550e60aed3e.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

-[Elk Install](https://github.com/TomoroCarpender/Cyber-Projects/blob/main/ansible/elk-playbook.rtf)

-DVWA

-[Filebeat](https://github.com/TomoroCarpender/Cyber-Projects/blob/main/ansible/filebeat/Filebeat-playbook.rtf)

-[Metricbeat](https://github.com/TomoroCarpender/Cyber-Projects/blob/main/ansible/metricbeat/metricbeat-playbook.rtf)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application. One vulnerability we looked at was when you are using the DVWA you can see the hostname of the machine you are working with and see this as the hostname is changed.  Once you can see this you can use DVMA to push Command Injection to a machine and gain access. We ran the following commands together to expose vulnerabilities in the network: 
-  ; runs a command after the first command, regardless of if the first command is successful.
-  && runs a command only if the first command is successful.
-  || runs a command only if the first command fails.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
This tool helps with Availability which is a key aspects of the CIA triad. The load balancer helps control the follow of traffic it also does diagnostics for the machines it is sending traffic to and if the machine has issues with functionality, it reports this and stops traffic to the machines.  It is affective for controlling DoS Attacks as it doesn't allow for any one server to be overloaded with traffic. 

The Jump Box can act as an intial stop for all traffic to route through which can limit the number of connection to the underlying network of machines. The Jump-box is a great tool for hardening and monitoring of the devices connected to a network.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Jump-box and system network.
-   I added Filebeat to monitor and forward log data this program looks for events in log and forwards them Elasticsearch with the help of Logstach. Filebeat is used to collect data about the file system. 
-  I also added Metricbreat which monitors the system server and sends metrics and statistics to Elasticsearch which again can be accessed using Elasticsearch with the help of Logstash.  Meticbeat is used to collect metrics and of machines.  One example of these metrics is uptime of a machine and times the machines might be being logged into more often. 

The configuration details of each machine may be found below.

| Name        | Function | IP Address | Operating System |
|---------      |----------  |------------|------------------|
Jump Box   | Gateway | 10.1.0.4            | Linux|
| Web 1       | Web Server | 10.1.0.8            | Linux               |
| Web 2       | Web Server  | 10.1.0.9            | Linux               |
| Web 3       | Web Server| 10.1.0.11          | Linux               |
| Elk-VM      | Elasticsearch Stack| 10.4.0.4    | Linux               |
    
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box machine can accept connections from the Internet. Access to this machine is only allowed frommy Machines IP address. 

I have created a rule in the Inbound Security Rules whitelisting my home machines IP only. 
Machines within the network can only be accessed by the Jump-box

The ELK server is logging Web 1, Web 2, and Web, 3 the data from the logging programs can only be accessed by using Public IP 13.67.186.145 and Private IP 10.4.0.4

A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible | Allowed IP Addresses |
|----------    |---------------------    |----------------------|
| Jump Box | SSH-22-Yes            | My public IP           |
|  Web 1      |  No                           |   LB - 13.82.18.172 |
|  Web 2      |  No                           |   LB -13.82.18.172  |
|  Web 3      |  Yes - HTTP- 80       |   LB -13.82.18.172  |
|  Elk-VM     |  Yes Kibana-5601    |  My public IP          |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because doing this you can limited the service running, system installation and updated is easier as well as more efficient and can be duplicated more easily. 

Most Security professionals have many tasks to perform and analysis to completed by automating with Ansible this task can be completed and duplicated more easily and frees up time for performing more urgent tasks for associates. 

The playbook implements the following tasks:
- Install Docker.io
- Install python3-pip
- Installs Docker module
- Increases virtual memory to 262144 - this increase in memory is needed to help Elk Docker launch as memory is a common problem which can cause issues with the overall launch of Elk Docker
- download and launch the docker elk container allowing only traffic the specific ports to limit traffic
published_ports:
         -  5601:5601
         -  9200:9200
         -  5044:5044
         
- Enable to docker to start on boot so that it runs each time the machine is booted and should not need to be started.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Image/Docker ps - Elk.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
 -Web 1     10.1.0.8
 -Web 2     10.1.0.9 
 - Web 3    10.1.0.11 

I have installed the following Beats on these machines:
- Filebeat
- Microbeat

The two beat will monitor and collect data such time of activity place activity originated from further I can get the host name and host id.  it will give me process name the PID id. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

Step 1 Copy the elk_install.yml file to /etc/ansible/roles folder

Step 2 Go into and updated the host file to include the attribute [elk] as well adding the destination ip of the ELK server. 
        [elk]
        10.4.0.4 ansible_python_insterpreter=/usr/bin/python

Step 3: Run the playbook and navigate to the elk container to check that the installation worked as expected.
               if this worked you should be able to go to  http://[you_elk_server_ip]:5601/app/kibana 

