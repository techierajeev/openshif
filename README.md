# openshift
This repository contains practical scenarios of deployment, managing resources, persistent volumes, config maps and all on openshift origin.
all practical implentation has been done on openshift origin version 3.6


let's start from installing openshift origin :

Running Openshift On Ubuntu

Using Release Download: -

step 1:- wget https://github.com/openshift/origin/releases/download/v1.4.1/openshift-origin-server-v1.4.1-3f9807a-linux-64bit.tar.gz
step 2:- tar xvzf openshift-origin-server-v1.4.1-3f9807a-linux-64bit.tar.gz
step 3:- mv openshift-origin-server-v1.4.1+3f9807a-linux-64bit/ openshift-origin
step 4:- cd openshift-origin
step 5:- sudo ./openshift start 

go to console using:- https://localhost:8443/console/
username:- admin
password:- admin


for details:-
source https://docs.openshift.org/latest/getting_started/administrators.html#installation-methods


Using Docker Image:-

sudo docker run -d --name "origin" \
--privileged --pid=host --net=host \
-v /:/rootfs:ro -v /var/run:/var/run:rw -v /sys:/sys -v /var/lib/docker:/var/lib/docker:rw \
-v /var/lib/origin/openshift.local.volumes:/var/lib/origin/openshift.local.volumes:rslave \
openshift/origin start


then go to console using https://localhost:8443/console/
username:- admin
password:- admin
