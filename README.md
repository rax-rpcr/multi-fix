# multi-fix

These scripts will build a basic two VM's running RDO. One node will be a controller node while the other will be a dedicated compute node. Purpose of these scripts is to build a lab to practice hands on break fix, maintenance preperation, or getting to know new features coming in RDO/OSP.

### Requirements for your machine:

### Virtualbox is required to build this lab:

 - **Virtualbox** ([https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads))
##### Directions to install Virtualbox
###### Fedora/CentOS/Red Hat:
	- All: `wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | rpm --import -`
	- RHEL/Centos: `http://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo`
	- Fedora: `http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo`
###### Ubuntu/Mint
	- Add this to /etc/apt/sources.list
	  - `deb http://download.virtualbox.org/virtualbox/debian xenial contrib`
	- then
	  - $ `wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -`
	  - $ `sudo apt-get update`
	  - $ `sudo sudo apt-get install virtualbox-5.1`

### Mandatory Software for your physical system (whichever virtualization engine you choose to use for your virtual machine):
 - **Git** ([https://git-scm.com/downloads](https://git-scm.com/downloads))
 - **Vagrant** ([https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html))
 - **Vagrant Sahara Plugin** (https://github.com/jedi4ever/sahara)
 - **Vagrant fog Plugin** (https://github.com/fog/fog-libvirt)
 - **Ansible** version 2 or newer ([http://docs.ansible.com/ansible/intro\_installation.html](http://docs.ansible.com/ansible/intro_installation.html))

### Resources required on your physical system if you use libvirt or VirtualBox:
 - 8196GB RAM
 - 3vcpu
 - 80GB disk

### Setup prior to deploy your first lab

###### Run the following commands with the user you are going to run your environment:
```
$ vagrant plugin install fog
$ vagrant plugin install sahara
```

### Deploying a Lab to test a maintenance or mess with new features
 
 1. Make sure all requirements are met above.
 2. `git clone http://github.com/rax-rpcr/multi-fix`
 3. `cd multi-fix`
 4. `./breakfix ./breakfix breaks/break000.yml`
    NOTE: This will deploy the lab, snapshot it, and apply some needed configuration... Nothing will be broken

### Deploying a Breakfix Lab:

 1. Make sure all requirements are met above.
 2. `git clone http://github.com/rax-rpcr/multi-fix`
 3. `cd multi-fix`
 4. Create a randomly selected breakfix by running:

    $ `./breakfix`

    OR select a specific breakfix exercise:

    $ `./breakfix breaks/break*XXX*.yml` (for example: `./breakfix breaks/break000.yml`)

 5. The first time you deploy your lab, the scripts are going to build the breakfix environment. Building the breakfix environment takes 20-30 minutes. Then, the scripts will take a snapshot and will rollback to it for the following exercises.
 6. The final output will inform you about where you can find info about the exercise.
 7. Capture/Document any needed information from the breakfix environment.

The individual breakfix playbooks are in the `breaks` sub-directory. Looking past the first few lines before solving the problem will spoil the exercise and bring you bad openstack karma.

When you are ready to try another exercise, just run the breakfix script again.  This will re-use a snapshot of the basic openstack installation created on the first run, so it will run much faster.

### Notes:

During the build it could happen that the CentOS repositories become unavailable or fails the OpenStack all-in-one installation. If that's the case, the script will retry to redeploy the ansible playbook until it's succesful. If something else besides that is broken, the script will exit. In such scenario, just rerun the script until the exit is succesfull and/or run `./breakfix-cleanup` if required.

### Cleaning up after you are done with all your labs:

Run "`./breakfix-cleanup`" to delete the VM and any snapshots.

### Want to participate? Adding a new breakfix exercise:

Create a new `breaks/breakXXX.yml` playbook file using the next available sequence number.  It is suggested that you copy an existing `break000.yml` (which is the current standard and contains the instructions how to build your own breaks) and modify it to implement your exercise.
