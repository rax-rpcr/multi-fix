# -*- mode: ruby -*-

# vi: set ft=ruby :

boxes = [
    {
        :name => "compute1",
        :eth1 => "192.168.56.20",
        :mem => "4096",
        #:mem => "2048",
        :cpu => "4"
    },
    #{
    #    :name => "compute2",
    #    :eth1 => "192.168.56.21",
    #    :mem => "4096",
    #    #:mem => "2048",
    #    :cpu => "4"
    #},
    {
        :name => "controller1",
        :eth1 => "192.168.56.10",
        :mem => "16384",
        #:mem => "6144",
        :cpu => "4"
    },
#    {
#        :name => "server3",
#        :eth1 => "192.168.205.12",
#        :mem => "2048",
#        :cpu => "2"
#    }
]

Vagrant.configure(2) do |config|

  #config.vm.box = "centos/7"
  config.vm.box = "CentOS-7-x86_64-Vagrant-1610_01.VirtualBox.box"
  config.vm.box_url = "http://cloud.centos.org/centos/7/vagrant/x86_64/images/CentOS-7-x86_64-Vagrant-1610_01.VirtualBox.box"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  #if Vagrant.has_plugin?("vagrant-cachier")
  # Configure cached packages to be shared between instances of the same base box.
  # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
  #  config.cache.scope = :box
  #  config.cache.auto_detect = true

  # OPTIONAL: If you are using VirtualBox, you might want to use that to enable
  # NFS for shared folders. This is also very useful for vagrant-libvirt if you
  # want bi-directional sync
  #config.cache.synced_folder_opts = {
  #  type: :nfs,
    # The nolock option can be useful for an NFSv3 client that wants to avoid the
    # NLM sideband protocol. Without this option, apt-get might hang if it tries
    # to lock files needed for /var/cache/* operations. All of this can be avoided
    # by using NFSv4 everywhere. Please note that the tcp option is not the default.
  #  mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
  #}
  # For more information please check http://docs.vagrantup.com/v2/synced-folders/basic_usage.html
  #end

  config.ssh.private_key_path = ["keys/private", "~/.vagrant.d/insecure_private_key"]
  config.ssh.forward_agent = true
  config.ssh.insert_key = false
  config.vm.network :forwarded_port, guest: 80, host: 8080, auto_correct: true

  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]

      config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--name", opts[:name]]
        v.customize ["modifyvm", :id, "--memory", opts[:mem]]
        v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
        # Enable pagefusion if you are running on Linux hosts
        v.customize ["modifyvm", :id, "--pagefusion", "on"]
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
    ansible.verbose = "vv"
    ansible.host_key_checking = false
  end
end
