## My Ansible Playbooks for Dev

1. Install ansible and virtualbox.
2. Clone this is to a roles directory then configure the project.
3. Then run vagrant up ...

## Example Vagrantfile

    Vagrant.configure('2') do |config|
      config.vm.box_url = 'http://cloud-images.ubuntu.com/vagrant/raring/current/raring-server-cloudimg-amd64-vagrant-disk1.box'
      config.vm.box = 'raring-server-cloudimg-amd64-vagrant-disk1.box'
      config.vm.network :private_network, ip: '192.168.1.1'

      config.vm.hostname = 'dev'

      config.vm.provider :virtualbox do |vb|
        vb.customize ['modifyvm', :id, '--name', 'dev', '--cpus', '1', '--memory', 512]
      end

      config.vm.provision :ansible do |ansible|
        ansible.playbook = 'site.yml'
      end
    end

## Example site.yml

    ---
    - include: dev.yml

## Example dev.yml

    ---
    - hosts: default
      sudo: yes
      roles:
        - common
        - vagrant
        - ide

Recent versions of vagrant will create a hosts file with the host default
defined. The only reason site.yml and dev.yml are two files and not one is so
that you might create a host file and a {production|staging|whatever}.yml and
use the same setup for real server provisioning. 

## TODO

1. Turn on query logging on the databases (for dev)
2. Figure out how to pass parameters to roles or use tags? to configure within a
   role.
