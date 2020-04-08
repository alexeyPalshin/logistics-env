# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"

  # to access es directly from the host machine
  config.vm.network "forwarded_port", guest: 9200, host: 9200

  config.vm.network "private_network", ip: "192.168.68.68"

  # synced folders
  config.vm.synced_folder "src/", "/home/vagrant/src", create: true
  
  config.vm.provider :virtualbox do |vb|
    vb.memory = 2048
    vb.cpus = 1  
  end

  config.vm.provision "file", source: "./configs", destination: "~/configs"
  config.vm.provision "file", source: "./Dockerfile", destination: "~/Dockerfile"
  config.vm.provision "file", source: "./docker-compose.yml", destination: "~/docker-compose.yml"
  config.vm.provision "file", source: "./up.sh", destination: "~/up.sh"

  Dir.glob("provisions/*.sh") do |file|
    config.vm.provision "shell" do |s|
      s.path = file
    end
  end  
  
  config.trigger.after :up do |trigger|
    trigger.info = "Start docker"
    trigger.run_remote = {inline: "bash /home/vagrant/up.sh"}
  end
  
end
