Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true
  config.vm.box = "ubuntu/jammy64"

  config.vm.define "appserver" do |h|
    h.vm.hostname = "appserver"
    h.vm.network "private_network", ip: "192.168.56.101"
  end

 config.vm.define "dbserver" do |h|
   h.vm.hostname = "dbserver"
   h.vm.network "private_network", ip: "192.168.56.102"
 end

 config.vm.define "dockervm" do |h|
   h.vm.hostname = "dockervm"
   h.vm.network "private_network", ip: "192.168.56.103"
 end

 config.vm.define "jenkinsvm" do |h|
   h.vm.hostname = "jenkinsvm"
   h.vm.network "private_network", ip: "192.168.56.104"
   h.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
   end
 end

 config.vm.define "kubernetesvm" do |h|
   h.vm.hostname = "kubernetesvm"
   h.vm.network "private_network", ip: "192.168.56.105"
   h.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
   end
 end

end
