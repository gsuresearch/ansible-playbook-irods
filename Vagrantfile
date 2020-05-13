Vagrant.configure("2") do |config|

  config.vm.define "box1" do |box1|
    box1.vm.box = "centos/7"
    box1.vm.network "public_network", bridge: 'enp0s25', ip: '192.168.1.60'
    box1.vm.provision "shell", inline: "/bin/cat /vagrant/authorized_keys.txt >> /home/vagrant/.ssh/authorized_keys"
  end

  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--memory", 2048]
  end

  config.vm.provision :ansible do |ansible|
     ansible.playbook = 'test.yml'
  end

end
