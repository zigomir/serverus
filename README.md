# Default Stack

Build me servers; Run them on [clouds](https://www.youtube.com/watch?v=3acIH2PhMe0).

## Usage

```sh
cp server_config.SAMPLE.yml server_config.yml
vagrant up
```


## DigitalOcean

You shouldn't have same SSH key already set on DigitalOcean. And be sure to generate API token with 
read and write permissions.

```sh
vagrant plugin install vagrant-digitalocean
vagrant up --provider=digital_ocean

ansible production -i hosts -m ping -u root
ansible-playbook -i hosts setup_digitalocean.yml
```

## Wrong Ruby version?

Be sure to re-install `bundler` and try to run `gem` cause executables might be referenced to `ruby1.9`.

```sh
update-alternatives --set ruby
```

## Source

ALl knowledge taken from [mazer-rackham](https://github.com/jlund/mazer-rackham) and [ansible](https://github.com/eduardodeoh/ansible).
