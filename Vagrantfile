servers = [
  { ip: '192.168.56.53', svrName: 'Icat', fwd: 0, setup: 1, dbName: 'ICAT', dbAcnt: 'irods', dbPwd: 'irods',
    irodsPkg: 'irods-server', dbPluginPkg: 'irods-database-plugin-postgres', svrType: 'icat', svcAcnt: 'irods', zoneKey: 'zoneKey', negKey: '12345678901234567890123456789012',
    ctrlKey: '12345678901234567890123456789012', iAcnt: 'rods', iPwd: 'rods', iZone: 'tempZone', iVault: '', iDBsvr: '127.0.0.1', iDBname: 'ICAT', iDBacnt: 'irods', iDBpwd: 'irods'},

  { ip: '192.168.56.54', svrName: 'Resc', fwd: 0, setup: 1, dbName: 'ICAT', dbAcnt: 'irods', dbPwd: 'irods',
    irodsPkg: 'irods-server', dbPluginPkg: 'irods-database-plugin-postgres', svrType: 'resc', svcAcnt: 'irods', zoneKey: 'zoneKey', negKey: '12345678901234567890123456789012',
    ctrlKey: '12345678901234567890123456789012', iAcnt: 'rods', iPwd: 'rods', iZone: 'tempZone', iVault: '', iDBsvr: '127.0.0.1', iDBname: 'ICAT', iDBacnt: 'irods', iDBpwd: 'irods',
    rescICAT: '192.168.56.53'}
]

ROLE_NAME = 'irods-'
BASE_NAME = 'test-' + ROLE_NAME

Vagrant.configure('2') do |config|

  config.vm.box = "centos/7"

  servers.each do |server|
    host_name = BASE_NAME + server[:svrName]

    config.vm.define host_name do |node|

      node.vm.hostname = host_name
      node.vm.box = "centos/7"
      #node.vm.network "public_network", bridge: 'enp0s25'
      node.vm.network 'private_network', ip: server[:ip]
      node.vm.provision "shell", inline: "/bin/cat /vagrant/authorized_keys.txt >> /home/vagrant/.ssh/authorized_keys"

      node.vm.provision 'ansible' do |ansible|
        ansible.playbook = 'test.yml'
        ansible.host_vars = {
          host_name => {"svrName" => server[:ip],
                      "fwd" => server[:fwd],
                      "setup" => server[:setup],
                      "dbName" => server[:dbName],
                      "dbAcnt" => server[:dbAcnt],
                      "dbPwd" => server[:dbPwd],
                      "irodsPkg" => server[:irodsPkg],
                      "dbPluginPkg" => server[:dbPluginPkg],
                      "svrType" => server[:svrType],
                      "svcAcnt" => server[:svcAcnt],
                      "zoneKey" => server[:zoneKey],
                      "negKey" => server[:negKey],
                      "ctrlKey" => server[:ctrlKey],
                      "iAcnt" => server[:iAcnt],
                      "iPwd" => server[:iPwd],
                      "iZone" => server[:iZone],
                      "iVault" => server[:iVault],
                      "iDBsvr" => server[:iDBsvr],
                      "iDBname" => server[:iDBname],
                      "iDBacnt" => server[:iDBacnt],
                      "iDBpwd" => server[:iDBpwd],
                      "rescICAT" => server[:rescICAT]
                      }
        }
      end
    end

    config.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--memory", 2048]
    end
  end
end


