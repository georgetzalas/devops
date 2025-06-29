To start up a VM.

vagrant up [available-vm]

Available vms

appserver
dbserver
dockervm
jenkinsvm
kubernetesvm

The jenkinsvm and kubernetesvm require manual installation of packages to work properly

jenkinsvm
-ansible
-jenkins
-jenkins freestyle job to download ansible playbooks from github
-jenkins freestyle job to download java application from github
-kubectl

kubernetesvm
-k8s

You also need hosts into your ~/.ssh/config

vagrant ssh-config [available-vm] >> ~/.ssh/config

You also need to pass the ssh keys onto the jenkins vms so it can connect to the deployment vms
or
you can connect with vagrant ssh [available-vm]

Steps

From your local machine take the private key generated for every vm created by vagrant using
cat .vagrant/machines/appserver/virtualbox/private_key (You must be in the directory that the .Vagrantfile is for this command to work)
do that for all the vms that you want the jenkins vm to be able to connect to

then

vagrant ssh jenkinsvm
sudo su jenkins
cd (go into home of jenkins user)
mkdir .keys
touch {vm}.key (do that for every key that you copied)
paste the key into {vm}.key (again  do that for all the keys that you copied)
also make change the permisions of the file chmod 600 {vm}.key

then 

mkdir .ssh
cd .ssh
touch config

in the config create a host for every vm for example

Host appserver
  HostName appserverip -> IP must be the same as the ip that the vagrant gave to the vm
  User vagrant
  Port 22
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /var/lib/jenkins/.keys/appserver.key -> Path to the .key files you created before
  IdentitiesOnly yes
  LogLevel FATAL
  PubkeyAcceptedKeyTypes +ssh-rsa
  HostKeyAlgorithms +ssh-rsa


Scenario 1 (execution steps)

Start vms:
    vagrant up appserver dbserver jenkinsvm

Access jenkins dashboard from your local system:
    http://192.168.56.104:8080/

Create an ansible-jenkins job (pipeline) and set it up to execute the ansible.Jenkinsfile from Distributed-System-Project-Team-22(github)

After that run the pipeline

You should be able to access the app at http://192.168.56.101

Scenario 2

Start vms:
    vagrant up dockervm jenkinsvm

Access jenkins dashboard from your local system:
    http://192.168.56.104:8080/

Create an docker-jenkins job (pipeline) and set it up to execute the ansible-compose.Jenkinsfile from Distributed-System-Project-Team-22(github)

After that run the pipeline

You should be able to access the app at http://192.168.56.103:8080


Scenario 3

Start vms:
    vagrant up kubernetesvm jenkinsvm

Access jenkins dashboard from your local system:
    http://192.168.56.104:8080/

Create an kubernetes job (pipeline) and set it up to execute the Jenkinsfile from Distributed-System-Project-Team-22(github)

After that run the pipeline

You should be able to access the app at http://192.168.56.105
