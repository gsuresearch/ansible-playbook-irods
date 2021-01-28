user = `whoami`
user = user.strip + '-irods'

servers = [
  { svrName: "#{user}-icat", playbook: 'deploy-irods.yml', template: 'centos/7', ip: '192.168.56.58', memory: '2048', vcpus: '1'},
  { svrName: "#{user}-resc", playbook: 'deploy-irods.yml', template: 'centos/7', ip: '192.168.56.59', memory: '2048', vcpus: '1' },
  { svrName: "#{user}-server", template: 'centos/7', ip: '192.168.56.62',  memory: '1024', vcpus: '1' }
]

# inventory = {
#       "beegfsClient" => ["#{user}-compute1", "#{user}-compute2"],
#       "beegfsClient:vars" => {"management_server_fqdn" => "192.168.56.65", "client_mount_point" => "/scratch"}
# }

host_vars = {
      "#{user}-icat" => { 'fwd' => '0', 'setup' => '1', 'dbName' => 'ICAT', 'dbAcnt' => 'irods', 'dbPwd' => 'irods', 'irodsPkg' => 'irods-server', 'dbPluginPkg' => 'irods-database-plugin-postgres',
                          'svrType' => 'icat', 'svcAcnt' => 'irods', 'zoneKey' => 'zoneKey', 'negKey' => '12345678901234567890123456789012', 'ctrlKey' => '12345678901234567890123456789012',
                          'iAcnt' => 'rods', 'iPwd' => 'rods', 'iZone' => 'tempZone', 'iVault' => '', 'iDBsvr' => '127.0.0.1', 'iDBname' => 'ICAT', 'iDBacnt' => 'irods', 'iDBpwd' => 'irods' },
      "#{user}-resc" => { 'fwd' =>  '0', 'setup' =>  '1', 'dbName' =>  'ICAT', 'dbAcnt' =>  'irods', 'dbPwd' =>  'irods', 'irodsPkg' =>  'irods-server', 'dbPluginPkg' =>  'irods-database-plugin-postgres',
                          'svrType' =>  'resc', 'svcAcnt' =>  'irods', 'zoneKey' =>  'zoneKey', 'negKey' =>  '12345678901234567890123456789012', 'ctrlKey' =>  '12345678901234567890123456789012',
                          'iAcnt' =>  'rods', 'iPwd' =>  'rods', 'iZone' =>  'tempZone', 'iVault' =>  '', 'iDBsvr' =>  '127.0.0.1', 'iDBname' =>  'ICAT',
                          'iDBacnt' =>  'irods', 'iDBpwd' =>  'irods', 'rescICAT' =>  '192.168.56.58' },
      "#{user}-server" => { 'var' => 'value'}
}

Vagrant.configure('2') do |config|

  servers.each do |server|

    host_name = server[:svrName]

    config.vm.define host_name do |node|

        node.vm.hostname = host_name
        node.vm.box = server[:template]
        node.vm.network 'private_network', ip: server[:ip]

        node.vm.provider :libvirt do |v|
            v.memory = server[:memory]
            v.cpus = server[:vcpus]

            if server.key?(:disks)
                server[:disks].each do |disk|
                    v.storage :file, :size => disk
                end
            end

        end

        node.vm.provision 'ansible' do |ansible|
            ansible.playbook = server[:playbook]
            ansible.host_vars = host_vars
            ansible.groups = inventory
        end
    end

  end
end