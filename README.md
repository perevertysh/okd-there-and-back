# okd-there-and-back
# installation guide of OKD version 3.11 to Ubuntu 20.04 

There are some problems with access to server from outer machines, because frontend jerk off local ip (127.0.0.1, realy?! wtf:)). It's like:
https://127.0.0.1/console/*redirect_to_login*

After installation can be problems with fetching git repos. Need to check access from containers or something else. Documentation on that odd job is too weak. 

Original guide you can watch on youtube (https://www.youtube.com/watch?v=eHrnGkegaQw).

# installing openshift-origin:

>user@oklahoma-city:~$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

>user@oklahoma-city:~$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

>user@oklahoma-city:~$ sudo apt update && sudo apt -y install docker-ce

>user@oklahoma-city:~$ sudo usermod -aG docker $USER

>user@oklahoma-city:~$ wget https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz

>user@oklahoma-city:~$ tar xvzf openshift*.tar.gz

>user@oklahoma-city:~$ cd openshift-origin-client-tools-*/

>user@oklahoma-city:~/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit$ sudo mv oc kubectl /usr/local/bin/

>user@oklahoma-city:~/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit$ sudo nano /etc/docker/daemon.json

add to file:
{ 
        "insecure-registries" : [ "172.30.0.0/16" ]
}

>user@oklahoma-city:~/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit$ sudo systemctl restart docker

>user@oklahoma-city:~/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit$ sudo systemctl status docker

>user@oklahoma-city:~/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit$ cd ..

at next steps, if there will be some problems with docker conf shut down docker, start docker again. also try to reboot machine**

>user@oklahoma-city:~$ sudo oc cluster up

>user@oklahoma-city:~$ sudo oc cluster down

>user@oklahoma-city:~$ ip address

check ip address of machine:
 net 192.168.0.106/24 brd 192.168.0.255 scope global dynamic noprefixroute enp0s3

>user@oklahoma-city:~$ sudo nano ./openshift.local.clusterup/openshift-controller-manager/openshift-master.kubeconfig

erase-paste ip address in that string:
><p>
        server: https://127.0.0.1:8443  to server: https://192.168.0.106:8443  !!!!!!!!!
</p>

>user@oklahoma-city:~$ sudo oc cluster up --public-hostname=192.168.0.106

success message is :
 OpenShift server started.
 The server is accessible via web console at:
     https://192.168.0.106:8443
