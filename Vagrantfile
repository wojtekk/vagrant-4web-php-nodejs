# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "opscode-ubuntu-12.04_chef-11.4.0"
  config.vm.box_url = "https://opscode-vm-bento.s3.amazonaws.com/vagrant/opscode_ubuntu-12.04_chef-11.4.0.box"

  config.vm.hostname = "myvm.dev"

  config.ssh.forward_agent = true

  config.vm.network "private_network", ip: "192.168.56.111"
  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.synced_folder "./", "/var/www"

  # Virtual Box
  config.vm.provider "virtualbox" do |my_vm|      
    my_vm.name = "myvm.dev" # VM name
    my_vm.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]
    # Core tools
    chef.add_recipe "apt"
    chef.add_recipe "vim"
    # Subversion, Git & Git Flow
    chef.add_recipe "git"
    chef.add_recipe "chef-git-flow"
    chef.add_recipe "subversion"
    # Apache & PHP
    chef.add_recipe "vhost"
    chef.add_recipe "php"
    chef.add_recipe "composer::install"
    chef.add_recipe "composer::self_update"
    chef.add_recipe "apache2::mod_php5"
    chef.add_recipe "apache2::mod_rewrite"
    chef.add_recipe "apache2::mod_expires"
    # Node & Npm
    chef.add_recipe "nodejs"
    chef.add_recipe "nodejs::npm"
    # Ruby & SASS
    chef.add_recipe "rubygems"
    chef.add_recipe "opscode-chef-cookbook-sass"

    chef.json = {
      :apache2 => {
        :contact              => "wojciech.krawczyk@firma.interia.pl",
        :timeout              => 30,
        :keepalive            => "On",
        :keepaliverequests    => 100,
        :keepalivetimeout     => 5,
        :vhosts               => [ {
          :name               => 'example.dev',
          :aliases            => [ 'www.example.dev' ],
          :ssl                => false,
          :environment        => 'development',
          :path               => "/var/www/public",
          :assets_path        => "/assets",
          :extra_php_filetype => [ '.html' ]
        } ]
      }
    }

  end

end
