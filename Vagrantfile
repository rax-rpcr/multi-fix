# -*- mode: ruby -*-

# vi: set ft=ruby :

boxes = [
    {
        :name => "compute1",
        :eth1 => "192.168.56.20",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "controller1",
        :eth1 => "192.168.56.10",
        :mem => "6144",
        :cpu => "2"
    },
#    {
#        :name => "server3",
#        :eth1 => "192.168.205.12",
#        :mem => "2048",
#        :cpu => "2"
#    }
]

Vagrant.configure(2) do |config|

  config.vm.box = "centos/7"

  config.ssh.private_key_path = ["keys/private", "~/.vagrant.d/insecure_private_key"]
  config.ssh.forward_agent = true
  config.ssh.insert_key = false
  config.vm.network :forwarded_port, guest: 80, host: 8080, auto_correct: true



  # Turn off shared folders
  config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]

      config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--name", opts[:name]]
        v.customize ["modifyvm", :id, "--memory", opts[:mem]]
        v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
        # Enable pagefusion if you are running on Linux hosts
        #v.customize ["modifyvm", :id, "--pagefusion", "on"]
        v.customize ["modifyvm", :id, "--paravirtprovider", "kvm"]
        v.customize ["modifyvm", :id, "--nictype1", "virtio"]
        v.customize ["modifyvm", :id, "--nictype2", "virtio"]
        v.customize ["modifyvm", :id, "--audio", "none"]
      end

      config.vm.network :private_network, ip: opts[:eth1]
    end
  end
  config.vm.provision "ansible" do |ansible|
    ansible.sudo = true
    ansible.playbook = "main.yml"
    ansible.verbose = "vvv"
    ansible.host_key_checking = false
  end
end
