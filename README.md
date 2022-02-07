## Kubernetes Multi-master Cluster With external etcd 


**Requirements**

3 servers( centos7) for Masters

3 server ( centos7) for Workers

1 server ( centos7) for loadbalancer

1 server ansible machine

VirtualBox

Install vagrant on your system


**Deploying Vagrant file to create machines**
  
 You should enter to vagrant directory and execute vagrantfile: 
  
   cd /vagrant
   
   vagrant up
   
**The setup:**

It contains a bunch of yml files which create structure of ansible provision and you can clone this and create a path in your ansible server and copy opennebula project in your provision directory in ansible server.All playbooks have their own path and being placed in tasks directories.

mkdir -p /home/ansible/provision

git clone https://github.com/sorooshmh/kubernetes.git


**Describe playbooks:**

**haproxy:**
It installs a loadbalancer which connect to Clusetr


    ansible-playbook haproxy.yaml -i inventory/haproxy -vv


**kubernetes-setup:**
This playbook use to create etcd and initialize the Multi-master Cluster


    ansible-playbook kubernetes-setup.yaml -i inventory/kubernetes-servers -vv



**prometheus-grafana:**

This playbook can create a monitoring stack with prometheus and grafana:


    ansible-playbook prometheus-grafana.yaml -i inventory/prometheus-grafana -vv
    
    
    
 
    
