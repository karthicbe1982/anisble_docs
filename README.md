

# Overview of Ansible
## What is Ansible?

Ansible is an open source automation platform. It's a simple automation language that can perfectly describe an IT application infrastructure in Ansible Playbooks. It's also an automation engine that runs Ansible Playbooks. ​	

Ansible can manage powerful automation tasks, and can adapt to many different workflows and environments. At the same time, new users of Ansible can very quickly use it to become productive.

 ### Ansible Is Simple

Ansible Playbooks provide human-readable automation. This means that your playbooks are automation tools that are also easy for humans to read, comprehend, and change. No special coding skills are required to write them. Playbooks execute tasks in order. The simplicity of playbook design makes them usable by every team. This allows people new to Ansible to get productive quickly.

 ### Ansible Is Powerful

You can use Ansible to deploy applications, for configuration management, for workflow automation, and for network automation. Ansible can be used to orchestrate the entire application life cycle.

 ### Ansible Is Agentless

Ansible is built around an agentless architecture. Typically, Ansible connects to the hosts it manages using OpenSSH or WinRM and runs tasks, often (but not always) by pushing out small programs called Ansible modules to those hosts. These programs are used to put the system in a specific desired state. Any modules pushed are removed when Ansible is finished with its tasks. It is possible to start using Ansible almost immediately, because no special agents need to be approved for use and then deployed to the managed hosts. Because there are no agents and no additional custom security infrastructure, Ansible is more efficient and more secure than other alternatives.

# Ansible Components & Architecture

 ### Control Nodes (Ansible-server)	

Ansible is installed and run from a control node, and this machine also has copies of your Ansible project files. A control node could be an administrator's laptop, a system shared by a number of administrators or a server running Ansible Tower. Control-node should have Linux OS.
 

### Managed Hosts (Ansible-Client)

Managed hosts are the slave nodes, which are connected to cntrol-node. managed-hosts can be more than one with any OS.
 

### Ansible.cfg:

This is the Ansible configuration file which manages the behaviour of ansible engine while communicatig with its inventory file and with the managed hosts.


### Inventory:

An inventory defines a collection of hosts that Ansible will manage. These hosts can also be assigned to groups, which can be managed collectively. Groups can contain child groups, and hosts can be members of multiple groups.
 

### Playbook:

Instead of writing complex scripts, Ansible users create high-level plays to ensure a host or group of hosts are in a particular state. A play performs a series of tasks on the host or hosts, in the order specified by the play. These plays are expressed in YAML format in a text file. A file that contains one or more plays is called a playbook.
 

### Ad-Hoc Commands:

An ad hoc command is a way to execute a single Ansible task quickly, one that you don't need to save to run again later. They're simple, one-line operations that can be run without writing a playbook.

The Ansible architecture is agentless. Typically, when an administrator runs an Ansible Playbook or an ad hoc command, the control node connects to the managed host using SSH (by default) or WinRM. This means that clients don't need to have an Ansible-specific agent installed on managed hosts.

### Modules:

Each task runs a module, a small piece of code (written in Python, PowerShell, or some other language), with specific arguments. Each module is essentially a tool in your toolkit. Ansible ships with hundreds of useful modules that can perform a wide variety of automation tasks. They can act on system files, install software.

When used in a task, a module generally ensures that some particular thing about the machine is in a particular state. For example, a task using one module may ensure that a file exists and has particular permissions and contents, while a task using a different module may make certain that a particular file system is mounted. If the system is not in that state, the task should put it in that state. If the system is already in that state, it should do nothing. If a task fails, Ansible's default behavior is to abort the rest of the playbook for the hosts that had a failure. Tasks, plays, and playbooks should be idempotent. This means that you should be able to run a playbook on the same hosts multiple times safely, and when your systems are in the correct state the playbook should make no changes when run.

 

    Architecture:

​	!Screenshot_20190226-150243~2](https://gagankubsad.github.io/ansible_docs/images/Screenshot_20190226-150243~2.png)

 
# ANSIBLE INSTALLATION:
Create Instances

For Ansible create 2 ec2 instances of RedHat in AWS. Name the instances as Control-Node & Managed-Host.

Control-Node == Ansible Server

Managed-Host == Ansible Client

 
SSH Connection

    Control-Node

#su - root

#ssh-keygen

#cd /root/.ssh

#cat id_rsa.pub

(copy the content of the file id_rsa.pub)

 

    Managed-Host 

#useradd devops

#passwd devops

#su - devops

#ssh-keygen

#su - root

#cp -rp /root/.ssh/authorized_keys /home/devops/.ssh

#chown devops:devops /home/devops/.ssh/authorized_keys

#vim /home/devops/.ssh/authorized_keys

(paste the id_rsa.pub file content which is copied from the control-node)

:wq!

 
Assigning SUDO Privileges

#vim /etc/sudoers

devops ALL=(ALL) NOPASSWD: ALL

:wq!

Installing Ansible Package

Ansible tool should be installed only on the control-node.

#yum install wget -y

#wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

#rpm -ivh epel-release-latest-7.noarch.rpm

#yum install ansible -y
Ansible.cfg

This is the Ansible configuration file which manages the behaviour of ansible engine while communicatig with its inventory file and with the managed hosts. ​	The behavior of an Ansible installation can be customized by modifying settings in the Ansible configuration file. Ansible chooses its configuration file from one of several possible locations on the control node.

 

    Configuration file locations and Precedence:

        Using ./ansible.cfg

​	(config file in present working directory) ​	If an ansible.cfg file exists in the directory in which the ansible command is executed, it is used instead of the global file or the user's personal file. This allows administrators to create a directory structure where different environments or projects are stored in separate directories, with each directory containing a configuration file tailored with a unique set of settings.

        Using ~/.ansible.cfg

​	(users personal file) ​	(config file in home directory of the user as a hidden file)

​	Ansible looks for a ~/.ansible.cfg in the user's home directory. This configuration is used if there is no ansible.cfg file in the present working directory.

        Using /etc/ansible/ansible.cfg

​	(global configuration file)

​	The ansible package provides a base configuration file located at /etc/ansible/ansible.cfg. This file is used if no other configuration file is found both in "present working directory(./ansible.cfg) and users home directory(~/.ansible.cfg)".

    Configuring Connections

Ansible needs to know how to communicate with its managed hosts. One of the most common reasons to change the configuration file is in order to control what methods and users Ansible will use to administer managed hosts. Some of the information needed includes:

a. Where the inventory is that lists the managed hosts and host groups. inventory = /opt/inventory

b. Which remote user to use on the managed hosts; this could be root or it could be an unprivileged user. remote_user = devops

c. Whether or not to prompt for an SSH password. ask_pass = true or ask_pass = false

d. whether to enable privilege_escalation or not. become = true or become = false

e. If the remote user is unprivileged, Ansible needs to know whether it should try to escalate privileges to root. become_user = root

f. how to escalate privileges to root. become_method = sudo

g. Whether or not to prompt for sudo password to gain privileges. become_ask_pass = false or become_ask_pass = true

        [defaults]

        inventory = /opt/inventory---------------->(a)

        remote_user = devops---------------------->(b)

        ask_pass = false-------------------------->(c)

        

        [privilege_escalation]

        become = true----------------------------->(d)

        become_method = sudo---------------------->(e)

        become_user = root------------------------>(f)

        become_ask_pass = false------------------->(g)

 
Inventory

An inventory defines a collection of hosts that Ansible will manage. These hosts can also be assigned to groups, which can be managed collectively. Groups can contain child groups, and hosts can be members of multiple groups.

Host inventories can be defined in two different ways:

    Static Inventory:
    Dynamiv Inventory: Static Inventory: 

 

    Static Inventory:

A static host inventory can be defined by a text file that specifies the managed hosts that Ansible targets. In its simplest form, a static inventory is a list of host names or IP addresses of managed hosts, each on a single line:​	

        web1.example.com

        web2.example.com

        db1.example.com

        db2.example.com

        192.0.2.42

        172.25.0.11

    ​Defining Host Groups

Normally, however, you organize managed hosts into host groups. Host groups allow you to more effectively run Ansible against a collection of systems. In this case, each section starts with a host group name enclosed in square brackets ([]). This is followed by the host name or an IP address for each managed host in the group, each on a single line.

In the following example, the host inventory defines two host groups, webservers and db-servers.

    [webservers]

    web1.example.com

    web2.example.com

    192.0.2.42

​

    [db-servers]

    db1.example.com

    db2.example.com

Hosts can be in multiple groups. In fact, recommended practice is to organize your hosts into multiple groups, possibly organized in different ways depending on the role of the host, its physical location, whether it's in production or not, and so on. This allows you to more easily apply Ansible plays to specific hosts.

    [webservers]

    web1.example.com

    web2.example.com

    192.168.3.7

    

    [db-servers]

    db1.example.com

    db2.example.com

    

    [production]

    web1.example.com

    web2.example.com

    db1.example.com

    db2.example.com

 

    Defining Nested Groups

Ansible host inventories can include groups of host groups. This is accomplished with the :children suffix. The following example creates a new group called "shopping-cart", which includes all of the hosts from the webservers and db-servers groups. ​	

        [webservers]

        web1.example.com

        web2.example.com

​

        [db-servers]

        db1.example.com

        db2.example.com

    

        [shopping-cart:children]

        webservers

        db-servers

 

    Simplifying Host Specifications with Ranges

Ansible host inventories can be simplified by specifying ranges in the host names or IP addresses. Numeric ranges can be specified, but alphabetic ranges are also supported. Ranges have the following syntax: ​	[START:END] ​	Ranges match all the values from START to END, inclusive. Consider the following examples: ​	

• 192.168.[4:7].[0:255] will match all IPv4 addresses in the 192.168.4.0/22 network (192.168.4.0 through 192.168.7.255). ​	

• server[01:20].example.com will match all hosts named server01.example.com through server20.example.com. ​	

• [a:c].dns.example.com will match hosts named a.dns.example.com, b.dns.example.com, and c.dns.example.com. ​	

• 2001:db8::[a:f] will match all IPv6 addresses from 2001:db8::a through 2001:db8::f.

If leading zeros are included in numeric ranges, they are used in the pattern. The second example above does not match server1.example.com but does match server07.example.com. To illustrate this, the following example uses ranges to simplify the usa and canada group definitions from the earlier example:

    [webservers]

    web[1:2].example.com

​

    [backup-servers]

    backup[01:02].example.com

 

    Testing the Inventory

When in doubt, test the machine's presence in the inventory with the ansible command:

[root@control-node ~]$ ansible web1.example.com --list-hosts

  hosts (1):

    web1.example.com

​

[root@control-node ~]$ ansible web01.example.com --list-hosts

  [WARNING]: provided hosts list is empty, only localhost is available

   hosts (0):

​

You can run the following command to list all hosts in a group:

​

[root@control-node ~]$ ansible webservers --list-hosts

  hosts (2):

    web1.example.com

    web2.example.com

 

    IMPORTANT:

If the inventory contains a host and a host group with the same name, the ansible command prints a warning and targets the host. The host group is ignored. ​	There are various ways to deal with this situation, the easiest being to ensure that host groups don't use the same names as hosts in the inventory.

 

    Dynamic Inventory: 

Ansible inventory information can also be dynamically generated, using information provided by external databases. The open source community has written a number of dynamic inventory scripts that are available from the upstream Ansible project. If those scripts don't meet your needs, you can also write your own.

 

 
