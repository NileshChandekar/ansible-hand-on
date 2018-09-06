### Ansible Configuration Management ### 

#### Features : 
 1) Single management Node - Unlimited number of managed nodes 
 2) Agent-less
  a) Note :An agent is available for improved communication : `fireball` 
 3) SSH-Based configuration 
  a) Note : This menas that nothing needs to be installed on managed node 
  b) NOTE : However, ensure password-less SSH is setup to minimize the connection overhead. 
 4) operates on 2 modes 
  a) Ad-hoc - one-off commands on targets : ALL or selected  
  b) Playbook (Multi-steps(Plays)) - 
 5) Provides Cloud supports - AWS, GCE(Google Cloud Engine), Rackspace, Linode, Openstack || Need of Restfull API to make communication 
 6) Hybrid, public and private implementations. 
 7) Ability to managed invontory regardless of geographic location. 
 8) Optionaly , there is Web Based Management tool via : Ansible Tower. 
  a) Job manager with log of everything. 
 9) Ansible does not run as a persistenet process , but rather , ad-hoc as needed. 
 NOTE : Ansible nature reflects a smaller footprint in that it does not need to run persitenently . 
10) Intelligent `pssh` wrapper (parallel ssh)


### Setup : Centos7-Ansible Management Node
#### 1) Installtion : 
```bash

	 # sudo yum install epel-release.noarch -y 
	 # yum install ansible.noarch ansible-doc.noarch pssh.noarch -y 
	 # rpm -qa |grep -i ansible
	 ~~~
	 ansible-doc-2.3.1.0-1.el7.noarch
	 ansible-2.3.1.0-1.el7.noarch
	 ~~~
	 # ps -ef |grep ansible 																// Ansbile did not ran any process, On the fly process only. 
	 # sudo ln -s /usr/share/doc/ansible-doc-2.3.1.0/ /var/www/html/ansible-doc 			// For offline ansbile documentation 
```

#### 2) Setup managed host - select subset of nodes to manage   || 2DB || 2 WEB || 
'''bash
	 # sudo vi /etc/ansible/hosts 
	~~~
	 ansiblebuild[1:2]
	 ansbiledb7db1
	 ansiblecentos7[1:2]
	 ansiblenagios1
	~~~

	 # sudo vi /etc/ansible/hosts
	~~~
	### 2017 Ansible Host Entry ### 
	192.168.122.40
	192.168.122.41
	192.168.122.42
	192.168.122.43
	192.168.122.44
	192.168.122.45
	~~~
	NOTE : Placing this nodes in an area is NOT under INI-styled header i.e [webserver], means they belongs by defaults to ALL. 
```

#### 3) Setup SSH-KEY on target (managed) nodes 
##### a) ` # ssh-copy-id target` 
##### b) ` # for ip in `cat /root/hostdetails` ; do ssh-copy-id -i ~/.ssh/id_rsa.pub $ip ; done				// loop per node - slower methos 
##### c) ` # cat ~/.ssh/id_rsa.pub | pssh -A -h "hostdetails" -l root -I "cat >> ~/.ssh/authorized_keys" ` - // -A =askpass , -I = send-input -h = hostfile

```bash
 	~~~
	vi hostdetails
	192.168.122.40
	192.168.122.41
	192.168.122.42
	192.168.122.43
	192.168.122.44
	192.168.122.45 
	~~~
```
##### d) ` # ssh root@192.168.122.40 ` 												// Lets try to SSH , expecting no password. Yahooo.....
##### e) ` # pssh  -h "hostdetails" -l root -i "uptime" `  							// Lets to some basic check whether password-less authentication is working or not. 
##### f) ` # pssh  -h "hostdetails" -l root -i "cat ~/.ssh/authorized_keys" ` 		// Lets Do some more test, check for authorized_keys copied or not. 

 NOTE : Now the SSH keys are setup properly, now we can use ansible , good to go 

### 4) Ansible general usage : 
 a) ` ansbile <pattern>(hostname | group | regex) [-m <module_name](default=commands) -a <argument> `
 NOTE : Why not simple use `pssh` for remote administartion, you can , however `pssh` lacks modules, with intelligence, to perform remote administartion. 


