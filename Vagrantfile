Vagrant.configure("2") do |config|
    config.vm.box = "raring64"
    config.vm.box_url = "http://cloud-images.ubuntu.com/raring/current/raring-server-cloudimg-vagrant-amd64-disk1.box"
    # eth1, this will be the endpoint
    config.vm.network :private_network, ip: "192.168.27.100"
    # eth2, this will be the OpenStack "public" network, use DevStack default
    config.vm.network :private_network, ip: "172.24.4.225", :netmask => "255.255.255.224", :auto_config => false
    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", 4096]
        vb.customize ["modifyvm", :id, "--cpus", 3]
        # vb.customize ["modifyvm", :id, "--cpuexecutioncap", 50]
       	# eth2 must be in promiscuous mode for floating IPs to be accessible
       	vb.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
        vb.customize ['createhd', '--filename', 'hdd_file.vdi', '--size', 1024 * 100, '--format', 'vdi']
        vb.customize ['storageattach', :id, '--storagectl', 'SATAController', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', "hdd_file.vdi"]
    end
    config.vm.provision :ansible do |ansible|
        ansible.playbook = "devstack.yaml"
        ansible.verbose = "vvvv"
    end
    config.vm.provision :shell, path: 'provision.sh'
end
