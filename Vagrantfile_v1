#Name to be used for the virtualbox__intnet network name and box name
domain = "kube"

#Define the boxes hostname, ip and id on an Array

nodes = [
    { :hostname => 'kmaster', :ip => '10.0.0.10', :id => '10' },
    { :hostname => 'node1', :ip => '10.0.0.11', :id => '11' },
    #{ :hostname => 'node2', :ip => '10.0.0.12', :id => '12' },
]

#defining variable for ram memory for the boxes 
#memory = 2000

#Shell script assinged to a global variable to be use in provisioning part
$script = <<-SCRIPT 
sudo mv hosts /etc/hosts
chmod 0600 /home/vagrant/.ssh
chmod 0700 /home/vagrant/.ssh
chown -R vagrant:vagrant /home/vagrant/.ssh
chmod 600 /home/vagrant/.ssh/authorized_keys
yum update -y
yum install -y python36
SCRIPT

##TODO: put pip install 

Vagrant.configure("2") do |config|
    #Tells vagrant not to mess up with ssh keys
    config.ssh.insert_key = false 
     # config.ssh.username = 'root'
    # config.ssh.password = 'vagrant'
    # config.ssh.insert_key = 'true'
    #For each node on the array setup it's configuration
    nodes.each do |node|
        ##### TODO: IF THE NODE IS THE MASTER SET SPECIAL SPECS ################
        if node[:hostname] == "kmaster"
            config.vm.define node[:hostname] do |nodeconfig|
                #image to use on each box
                nodeconfig.vm.box = "ol7-latest"
                nodeconfig.vm.box_url = "https://yum.oracle.com/boxes/oraclelinux/latest/ol7-latest.box"
                nodeconfig.vm.hostname = node[:hostname]
                #Specifying the network type and name
                nodeconfig.vm.network :private_network, ip: node[:ip], virtualbox__intnet: domain
                nodeconfig.vm.provider :virtualbox do |vb|
                    vb.name = node[:hostname]+"."+domain
                    vb.memory = 2500
                    vb.cpus = 2
                    #Virtual box vbmanage custom directives, prior to boot of each box
                    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
                    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
                    vb.customize ['modifyvm', :id, '--macaddress1', "5CA1AB1E00"+node[:id]]
                    vb.customize ['modifyvm', :id, '--natnet1', "192.168/16"]
                end
                #using local hosts file on this directory to set it up on each box
                nodeconfig.vm.provision "file", source: "hosts", destination: "hosts"
                #Sets the local ssh key provided by vagrant to each box under the vagrant user's .ssh folder
                nodeconfig.vm.provision "file", source: "/Users/dabenson/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/id_rsa"
                #Executes the containing script multilines on the $script variable
                nodeconfig.vm.provision "shell", inline: $script
            end
        else
            config.vm.define node[:hostname] do |nodeconfig|
                nodeconfig.vm.box = "ol7-latest" #image to use on each box
                nodeconfig.vm.box_url = "https://yum.oracle.com/boxes/oraclelinux/latest/ol7-latest.box"
                nodeconfig.vm.hostname = node[:hostname]
                # nodeconfig.vm.forward_port 8080, 31223
                # nodeconfig.vm.forward_port 3000, 31125
                #Specifying the network type and name
                nodeconfig.vm.network :private_network, ip: node[:ip], virtualbox__intnet: domain
                nodeconfig.vm.provider :virtualbox do |vb|
                    vb.name = node[:hostname]+"."+domain
                    vb.memory = 1000
                    vb.cpus = 1
                    #Virtual box vbmanage custom directives, prior to boot of each box
                    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
                    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
                    vb.customize ['modifyvm', :id, '--macaddress1', "5CA1AB1E00"+node[:id]]
                    vb.customize ['modifyvm', :id, '--natnet1', "192.168/16"]
                end
                #using local hosts file on this directory to set it up on each box
                nodeconfig.vm.provision "file", source: "hosts", destination: "hosts"
                #Sets the local ssh key provided by vagrant to each box under the vagrant user's .ssh folder
                nodeconfig.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/id_rsa"
                #Executes the containing script multilines on the $script variable
                nodeconfig.vm.provision "shell", inline: $script
            end
        end
    end
end
