# OpenShift Dynamic Volume Provisioning using heketi

This Repository Contains Steps involved in Dynamic Volume Provisioning using gluster as PV-plugins through heketi :-

I assume before going through this doc you must have the basic idea about the heketi and glusterfs, and also about Persistent Volume Concept in kubernetes or openshift 

In this example i am having openshift cluster with 3 master and 5 nodes (all centos machine 7.4 ), gluster server and heketi are running dedicated on the machine not as a container.

Step 1: first we need to setup gluster cluster , in gluster-cluster comprises all the 3 master ,
	steps to install and setup gluster cluster :-


	1: yum install centos-release-gluster312 -y
	2: yum install glusterfs-server
	3: systemctl restart glusterd
	4: systemctl status glusterd
	5: systemctl enable glusterd	


    These 5 commands must be executes on all the nodes which will be in gluster cluster

Step 2: Disable the firewall


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

those who already have their cluster cluster ready can skip step 1 and step 4

now starts the heketi part.  

Step 5: install heketi and heketi-cli
	



