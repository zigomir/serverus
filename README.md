# Default Stack

Build me servers; Run them on [clouds](https://www.youtube.com/watch?v=3acIH2PhMe0).

## Usage

```sh
cp server_config.SAMPLE.yml server_config.yml
vagrant up
```


## DigitalOcean

You shouldn't have same SSH key already set on DigitalOcean. And be sure to generate auth token with 
read and write permissions.

```sh
vagrant plugin install vagrant-digitalocean
vagrant up --provider=digital_ocean

ansible production -i hosts -m ping -u root
ansible-playbook -i hosts setup_digitalocean.yml
```

## Extra steps

Be sure to re-install `bundler` and try to run `gem` cause executables might be referenced to `ruby1.9`.

As root user

```sh
ssh -T git@github.com # add ~/.ssh/id_rsa.pub key to GitHub and re-run this command

update-alternatives --set ruby /usr/bin/ruby2.1
update-alternatives --set gem /usr/bin/gem2.1

sudo adduser deployer sudo
visudo

# Add following lines beneath %sudo   ALL=(ALL:ALL) ALL
deployer    ALL= (root) NOPASSWD:   /sbin/start api, /sbin/stop api, /sbin/restart api, /sbin/status api
deployer    ALL= (root) NOPASSWD:   /sbin/start auth, /sbin/stop auth, /sbin/restart auth, /sbin/status auth
```

Debugging

```sh
cd /var/log/upstart
tail -f api.log
tail -f auth.log
```

Be sure to comment this launch section out from mina's `deploy.rb` out for the first time. 
Because there is no unicorn.rb script in config directory which is needed by upstart script

As deployer user

```sh
touch ~/api/shared/tmp/pids/unicorn.pid
touch ~/auth/shared/tmp/pids/unicorn.pid
```

## Source

ALl knowledge taken from [mazer-rackham](https://github.com/jlund/mazer-rackham) and [ansible](https://github.com/eduardodeoh/ansible).
