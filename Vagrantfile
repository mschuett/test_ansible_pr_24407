
Vagrant.configure("2") do |config|
  # basebox
  config.vm.box="bento/centos-7.3"

  # virtualbox config (default)
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus   = 1
  end

  # workaround for a bug with Vagrant 1.9.1 and CentOS/RHEL 7.x
  # cf. https://github.com/mitchellh/vagrant/issues/7995
  #     https://github.com/mitchellh/vagrant/issues/8096
  config.vm.provision "shell",
                      name: "fix_private_network_if",
                      inline: "ifconfig enp0s8 | grep -q 'inet ' || sudo /sbin/ifup enp0s8"

  config.vm.define "zabbix" do |node|
    node.vm.hostname = "zabbix"
    node.vm.network "private_network", ip: "192.168.56.202"
    # zabbix web access
    node.vm.network "forwarded_port", guest: 80, host: 8000, auto_correct: true
  end

  # define one host as workstation, this one will run Ansible and possibly Jenkins.
  # it is also useful to have this a Linux test environment for scripts etc.
  # cf. controller example on https://www.vagrantup.com/docs/provisioning/ansible_local.html
  #
  # This has to be the last VM definition!
  config.vm.define "workstation", primary: true do |node|
    node.vm.hostname = "workstation"
    node.vm.network "private_network", ip: "192.168.56.200"

    node.vm.provision :ansible_local do |ansible|
      ansible.install_mode     = "pip"
      ansible.version          = "2.3.2.0"

      ansible.playbook         = "playbook.yml"
      ansible.limit            = "all"
      ansible.inventory_path   = "inventory"
      ansible.galaxy_role_file = "requirements.yml"
    end
  end
end
