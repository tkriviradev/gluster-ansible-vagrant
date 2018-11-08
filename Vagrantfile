# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Base VM OS configuration.
  config.vm.box = "centos/7"
  #config.ssh.insert_key = false
  config.vm.synced_folder "ansible/", disabled: true
  config.vm.provision :shell, path: "bootstrap.sh"
  ssh_pub_key = File.readlines("ssh/tsvyatko_id_rsa.pub").first.strip
  config.vm.provision 'shell', inline: 'mkdir -p /root/.ssh'
  config.vm.provision 'shell', inline: "echo #{ssh_pub_key} >> /root/.ssh/authorized_keys"
  config.vm.provision 'shell', inline: "echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys", privileged: false

  config.vm.provider :virtualbox do |v|
    v.memory = 1024
    v.cpus = 1
  end

  # Define two VMs with static private IP addresses.
  boxes = [
    { :name => "gluster1", :ip => "192.168.29.2" },
    { :name => "gluster2", :ip => "192.168.29.3" },
    { :name => "gluster3", :ip => "192.168.29.4" },
    { :name => "glusterd", :ip => "192.168.29.5" }
  ]

  # Provision each of the VMs.
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:ip]

      # Provision both VMs using Ansible after the last VM is booted.
      #if opts[:name] == "gluster3"
      #  config.vm.provision "ansible" do |ansible|
      #    ansible.playbook = "playbooks/provision.yml"
      #    ansible.inventory_path = "inventory"
      #    ansible.limit = "all"
      #  end
      #end
    end
  end

end