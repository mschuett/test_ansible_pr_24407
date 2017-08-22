
Vagrant.configure("2") do |config|
  # basebox
  config.vm.box="bento/centos-7.3"

  # virtualbox config (default)
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus   = 1
  end

  config.vm.define "zabbix" do |node|
    node.vm.hostname = "zabbix"
    node.vm.network "private_network", ip: "192.168.56.202"
    # zabbix web access
    node.vm.network "forwarded_port", guest: 80, host: 8000, auto_correct: true
  end

  # define one host as workstation to run Ansible
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
