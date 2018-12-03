# k8_vagrant_ansible_deploy

This is a automated deploy of a 3 node cluster running ubuntu/xenial minimal distro.

#Prerequisites localhost running this setup

1. Install Vagrant
2. Install Ansible
3. Install VirtualBox

#To specify the number of nodes, edit the Vagrant file section: 

    nodes = [
        { :hostname => 'kmaster', :ip => '10.0.0.10', :id => '10' },
        { :hostname => 'node1', :ip => '10.0.0.11', :id => '11' },
        { :hostname => 'node2', :ip => '10.0.0.12', :id => '12' },
    ]

#Change the local vagrant ssh key path: 

    config.ssh.private_key_path = "/Users/dabenson/.vagrant.d/insecure_private_key"


#Edit the VM's specs:

Edit the following on the Vagrant file, according to standards, I am leaving 2 cpu and 2 GB memory for the master node, but feel free to edit this value according your hardware.

    vb.memory = 2000
    vb.cpus = 2

#Run the setup 

vagrant up 


#Ansible post provisioning

Comming soon....