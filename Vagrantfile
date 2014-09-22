require 'yaml'

CONF = YAML.load_file(File.join(__dir__, 'server_config.yml'))

Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/trusty64'

  CONF['forward_ports'].each do |port|
    config.vm.network :forwarded_port, guest: port['guest'], host: port['host']
  end

  config.vm.synced_folder '.', '/vagrant', type: 'nfs'
  config.vm.network :private_network, ip: CONF['private_ip']

  config.vm.provider :virtualbox do |provider, override|
    provider.customize ['modifyvm', :id, '--name',   CONF['name']]
    provider.customize ['modifyvm', :id, '--memory', CONF['memory']]
  end

  config.vm.provider :digital_ocean do |provider, override|
    override.ssh.private_key_path = '~/.ssh/id_rsa'
    override.vm.box               = 'digital_ocean'
    override.vm.box_url           = 'https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box'

    provider.token  = CONF['digital_ocean']['token']
    provider.image  = CONF['digital_ocean']['image']
    provider.region = CONF['digital_ocean']['region']
    provider.size   = CONF['digital_ocean']['size']
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'setup.yml'
    ansible.inventory_path = 'hosts'
    ansible.limit = 'all'
  end
end
