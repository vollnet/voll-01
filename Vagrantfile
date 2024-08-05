# -*- mode: ruby -*-
# vi: set ft=ruby :

# *** Atenção!! ********************************************************************************
#
#    Após a instalação do aplicativo Vagrant na máquina host executar o plugin facilitador de
# conexões de rede local (VMs <> Host ou mesmo VMs <> VMs). 
#
#   $ vagrant plugin install vagrant-vbguest
#
#   $ vagrant plugin list
# 
# **********************************************************************************************
#
# -*- mode: ruby -*-
# vi: set ft=ruby :
#

#
# Include VBox configuration from YAML file 
#
require 'yaml'
settings = YAML.load_file 'vagrant.yml'

Vagrant.configure("2") do |config|
    #
    # Esta configuração é genérica, válida para todas as vm's
    #

    # Check local boxes from Hashcorp:
    #
    # $ vagrant box list
    #

    # Vbox Update runtime:
    config.vm.box_check_update = false

    # Network Interface VBox configuration:
    config.vm.network "public_network", ip: settings['ip_address'], bridge: settings['name_bridge']
 
    #
    # define vbox build 
    #
    config.vm.define settings['app_name'] do |app_config|
      #
      # vm-vagrant distro: Official Ubuntu LTS
      app_config.vm.box = settings['type_vbox']

      #
      # Sync App Folder
      #
      app_config.vm.synced_folder "./sync_folder_host", "/home/vagrant/sync_folder_work", create: true
 
      # define hostname: developer, operation, tester, homolog,  production
      #
      app_config.vm.hostname = settings['vm_hostname']
      #
      
      #
      app_config.ssh.insert_key = false
      #

      #
      app_config.vm.provider "virtualbox" do |ap|
        #
        # define tamanho da memória RAM da VM-WEB
        #
        ap.customize ["modifyvm", :id, "--memory", "1024"]
        ap.customize ["modifyvm", :id, "--cpus", 2]
        #
        # VBox Name:
        #
        ap.name = settings['vm_name']
      end
      #
      # Atualização & Provisionamento
      #
      app_config.vm.provision :shell, inline: "echo Provisionamento da vbox"
      #
      app_config.vm.provision "ansible" do |ansible|
        ansible.playbook =   settings['ansible_path']  
        ansible.verbose = "v"
      end
      #
      #
    end
    #
    #
  end