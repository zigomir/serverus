require 'yaml'

CONF = YAML.load_file('server_config.yml')

Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/trusty64'

  config.vm.provider :virtualbox do |provider, override|
    CONF['forward_ports'].each do |port|
      override.vm.network :forwarded_port, guest: port['guest'], host: port['host']
    end

    CONF['shared_dires'].each do |dir|
      override.vm.synced_folder dir['guest'], dir['host'], type: 'nfs'
    end

    # You need both
    # Be sure to run development servers bound to 0.0.0.0 (rails s -b 0.0.0.0 | rackup -o 0.0.0.0)
    override.vm.network :private_network, ip: CONF['private_ip']
    override.vm.network :public_network,  bridge: 'en0: Wi-Fi (AirPort)'
    # provider.gui = true # Helps a lot when problems with Vagrant occur debug

    # Greath SSH + Vagrant source: http://www.phase2technology.com/blog/running-an-ssh-agent-with-vagrant/
    # be sure to eval $(ssh-agent)
    # ssh-add
    # ssh-add -l # to see if there is identity added to ssh agent
    override.ssh.forward_agent = true

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
