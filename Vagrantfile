# -*- mode: ruby -*-
# vim: ft=ruby


# ---- Configuration variables ----

GUI               = false # Enable/Disable GUI
RAM               = 8192   # Default memory size in MB

# Network configuration
DOMAIN            = ".osp.example.com"
NETWORK           = "192.168.56."
NETMASK           = "255.255.255.0"

# Default Virtualbox .box
# See: https://wiki.debian.org/Teams/Cloud/VagrantBaseBoxes
BOX               = 'centos/7'


HOSTS = {
   "compute1" => [NETWORK+"20", RAM, GUI, BOX],
   "controller1" => [NETWORK+"10", RAM, GUI, BOX],
}

#ANSIBLE_INVENTORY_DIR = 'ansible/inventory'

# ---- Vagrant configuration ----

Vagrant.configure(2) do |config|
  config.ssh.private_key_path = ["keys/private", "~/.vagrant.d/insecure_private_key"]
  config.ssh.forward_agent = true
  config.ssh.insert_key = false

  config.vm.network :forwarded_port, guest: 80, host: 8080, auto_correct: true

  HOSTS.each do | (name, cfg) |
    #ipaddr, ram, gui, cpu, box = cfg
    ipaddr, ram, gui = cfg

    config.vm.define name do |machine|
      machine.vm.box   = "centos/7"
      #machine.vm.guest = :centos

      machine.vm.provider "virtualbox" do |vbox|
        vbox.gui = gui
        vbox.memory = ram
        vbox.name = name
        vbox.cpus = 2
      end

      machine.vm.hostname = name + DOMAIN
      machine.vm.network 'private_network', ip: ipaddr, netmask: NETMASK
    end
  end # HOSTS-each
  config.vm.provision "file", source: "keys/public", destination: "~/.ssh/authorized_keys"

  config.vm.provision "ansible" do |ansible|
    ansible.sudo = true
    ansible.playbook = "main.yml"
    ansible.verbose = "vvv"
    ansible.host_key_checking = false
  end

end
