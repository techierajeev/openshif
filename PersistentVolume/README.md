# OpenShift Dynamic Volume Provisioning using heketi

This Repository Contains Steps involved in Dynamic Volume Provisioning using gluster as PV-plugins through heketi :-

I assume before going through this doc you must have the basic idea about the heketi and glusterfs, and also about Persistent Volume Concept in kubernetes or openshift 

In this example i am having openshift cluster with 3 master and 5 nodes (all centos machine 7.4 ), gluster server and heketi are running dedicated on the machine and not as a container.

Prerequisite :

	1: password less login to all nodes in the glusterfs cluster from heketi node.

Step 1: first we need to setup gluster cluster,
	steps to install and setup gluster cluster :-


	1: yum install centos-release-gluster312 -y
	2: yum install glusterfs-server
	3: systemctl restart glusterd
	4: systemctl status glusterd
	5: systemctl enable glusterd	


    These 5 commands must be executes on all the nodes which will be in gluster cluster

Step 2: Disable the firewall/Bypass the services


	1: systemctl status  firewalld (if running)
	2: systemctl stop firewalld
	3: systemctl disable firewalld

Step 3: check the status of selinux

	1: getenforce
	  if it is in disbled mode then its okay, if enforcing/permissive then :
	vi /etc/selinux/config 
	change SELINUX=disabled
	2: setenforce 0

Step 4: now go to any of one machine of gluster and execute the following command:
	
	1: gluster peer probe ${IPADDR}
	add the ip of all the machines which you want to keep in the cluster.

	2: check the gluster peer info
	  gluster peer status

	other hosts must be in connected state

those who already have their gluster cluster ready can skip step 1 and step 4

now starts the heketi part.  

Step 5: install heketi and heketi-cli
	
	1: yum install heketi heketi-client -y
	
Step 6: configuring Heketi

	1: cd /etc/heketi/
	2: go through heketi.json file and configure it accordingly
	3: get the sample topology.json file from repository and modify it accordingly.
	4: systemctl restart heketi
	5: systemctl enable heketi

Step 7: checking if heketi is creating volumes:-

	1: export HEKETI_CLI_SERVER=${IP}:8080
	2: heketi-cli volume create --size 1

Step 8: if volume creation suceeds then check the /etc/fstab file all volumes created and mount point entry must be there. Do not restart/power off the machine in between volume creation/deletion commands executes, if fstab entry don't get remived properly then machine may crash after reboot.

Step 9: create the storage class and Secret if you are enabling authentication

	1: oc create -f <storageclassfile>.yml
	2: oc create -f <secretfile>.yml

Step 10: make the pvc request , pv will be created dynamically and volume will get mountes to it.
 
