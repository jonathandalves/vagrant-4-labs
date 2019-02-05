# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'

yaml = YAML.load_file("machines.yml")

Vagrant.configure("2") do |config|
  yaml.each do |server|
    config.vm.define server["name"] do |srv|
      srv.vm.box = server["sistema"]
      srv.vm.network "private_network", ip: server["ip"]
      srv.vm.hostname = server["hostname"]
      srv.vm.provider "virtualbox" do |vb|
        vb.name = server["name"]
        vb.memory = server["memory"]
        vb.cpus = server["cpus"]
    end

    if server["sistema"] == "ubuntu/bionic64"
      srv.vm.provision "shell", inline: "apt install python -y"
    else
      config.vm.provision "shell", inline: "mkdir -p /root/.ssh"
    end

      config.vm.provision "shell", inline: "cp /vagrant/id_rsa /root/.ssh/id_rsa"
      config.vm.provision "shell", inline: "cp /vagrant/id_rsa.pub /root/.ssh/authorized_keys"

     srv.vm.provision :ansible do |ansible|
       ansible.limit = "all"
       ansible.compatibility_mode = "2.0"
       ansible.playbook = "playbook.yml"
     end
  end
end

end
