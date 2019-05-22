# -*- mode: ruby -*-
NUMBER_OF_WEBSERVERS = 2
CPU_WEB = 2
CPU_LB = 2
MEMORY = 1024
ADMIN_USER = "vagrant"
ADMIN_PASSWORD = "vagrant"
VM_BOX= "bento/ubuntu-18.04"
VM_BOX_VERSION = ">= 201812.27.0"
#VM_VERSION= "https://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"
VAGRANT_VM_PROVIDER = "virtualbox"
LOAD_BALANCER_IP = "192.168.1.170"

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  groups = {
    "webservers" => ["web[1:#{NUMBER_OF_WEBSERVERS}]"],
    "loadbalancers" => ["load_balancer"],
    "all_groups:children" => ["webservers","loadbalancers"]
  }

  # create some web servers
  # https://docs.vagrantup.com/v2/vagrantfile/tips.html
  (1..NUMBER_OF_WEBSERVERS).each do |i|
    config.vm.define "web#{i}" do |node|
        node.vm.box = VM_BOX
        node.vm.box_version = VM_BOX_VERSION
        node.vm.hostname = "web#{i}"
#        node.vm.network :private_network, ip: "10.0.15.2#{i}"
#        node.vm.network :public_network, ip: "192.168.1.17#{i}", bridge: "Intel(R) 82579V Gigabit Network Connection"
        node.vm.network :public_network, ip: "192.168.1.17#{i}"
        node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
        node.vm.network "forwarded_port", guest: 22, host: "2222#{i}", guest_ip: "192.168.1.17#{i}"
        node.vm.provider VAGRANT_VM_PROVIDER do |vb|
          vb.name = "web#{i}"
          vb.memory = MEMORY
          vb.cpus = CPU_WEB
        end

    # Only execute once the Ansible provisioner,
    # when all the machines are up and ready.

# Can un-comment after ansible is installed to Cygwin
# but may not need since Trellis will provision web servers
#      if i == NUMBER_OF_WEBSERVERS
#          node.vm.provision "ansible" do |ansible|
#            ansible.playbook = "pb_web.yml"
#            ansible.sudo = true
#            ansible.limit = "all"
#            ansible.groups = groups
#          end
#        end

      end
    end


    # create load balancer
#    config.vm.define "load_balancer" do |lb_config|
#        lb_config.vm.box = VM_BOX
#        lb_config.vm.box_version = VM_BOX_VERSION
#        lb_config.vm.hostname = "lb"
##        lb_config.vm.network :private_network, ip: "10.0.15.11"
##        lb_config.vm.network :public_network, ip: "192.168.1.170", bridge: "Intel(R) 82579V Gigabit Network Connection"
#        lb_config.vm.network :public_network, ip: "192.168.1.170"
#        lb_config.vm.network "forwarded_port", guest: 80, host: 8011
#        lb_config.vm.network "forwarded_port", guest: 22, host: 22, guest_ip: LOAD_BALANCER_IP
#        lb_config.vm.provider VAGRANT_VM_PROVIDER do |vb|
#          vb.name = "load_balancer"
#          vb.memory = MEMORY
#          vb.cpus   = CPU_LB
#        end
## Can un-comment after ansible is installed to Cygwin
## but may not need since Trellis will provision web servers
##        lb_config.vm.provision "ansible" do |ansible|
##          ansible.playbook = "pb_lb.yml"
##          ansible.sudo = true
##          ansible.groups = groups
##        end

#    end
end
