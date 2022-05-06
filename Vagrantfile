# -*- mode: ruby -*-
# vi: set ft=ruby :

require "rbconfig"

Vagrant.configure("2") do |config|

    config.vagrant.plugins = [ "vagrant-env" ]

    config.env.enable
    UISP_VERSION = ENV["UISP_VERSION"] || "1.4.3"
    ROOT_PASSWORD = ENV["ROOT_PASSWORD"] || "vagrant"

    # Set the VM's network type to private and assign a static IP address.
    config.vm.network "private_network", ip: "192.168.50.10"

    # Forward the necessary ports to the box.
    config.vm.network "forwarded_port", guest: 80, host: 80, host_ip: "127.0.0.1"
    config.vm.network "forwarded_port", guest: 443, host: 443, host_ip: "127.0.0.1"

    # Including our newly exposed PostgreSQL port.
    config.vm.network "forwarded_port", guest: 5432, host: 5432, host_ip: "127.0.0.1"

    # Disable the default synced folder.
    config.vm.synced_folder ".", "/vagrant", disabled: true

    # Synchronize our docker-composer overrides.
    config.vm.synced_folder "./app", "/home/unms/app",
        type: "rsync",
        rsync__args: ["-r", "--include=docker-compose.override.yml", "--exclude=*"],
        owner: "unms", # 1001
        group: "root",
        create: true

    # Synchronize our custom UCRM (w/ Xdebug) docker image files.
    config.vm.synced_folder "./app/xdebug/", "/home/unms/app/xdebug/",
        type: "rsync",
        owner: "unms", # 1001
        group: "root",
        create: true

    # Synchronize a folder specifically for pushing sensitive information back from the guest system.
    config.vm.synced_folder "./env/", "/home/vagrant/env/",
        type: "smb"

    # Synchronize the "plugins" folder that maps to UCRM: /data/ucrm/data/plugins/
    config.vm.synced_folder "./src/", "/home/vagrant/src/",
        type: "smb"

    # Synchronize the "routers" folder that maps to UCRM: /usr/src/ucrm/web/_plugins/
    config.vm.synced_folder "./www/", "/home/vagrant/www/",
        type: "smb"

    # Set the base to our custom UISP Vagrant box.
    config.vm.box = "ucrm-plugins/uisp"
    config.vm.box_version = "#{UISP_VERSION}"

    # VirtualBox VM Configuration.
    config.vm.provider "virtualbox" do |vm|
        vm.name = "uisp-dev-#{UISP_VERSION}"

        # NOTE: Set the following to suit your needs and based upon available host resources.
        vm.cpus = 1
        vm.memory = 4096
    end

    # Provision the VM.
    config.vm.provision "shell", keep_color: true, inline: <<-SHELL

        # Change the root password.
        echo "root:#{ROOT_PASSWORD}" | sudo chpasswd

        # Pre-create the necessary folders for mounting and set any starting permissions/ownership.
        mkdir -p /home/vagrant/{src,www}/ && chown -R vagrant:vagrant /home/vagrant/{src,www}/

        # Build (forced) and run our modified UCRM Docker Container.
        cd /home/unms/app && sudo docker-compose -p unms up -d --build

        # Copy and sensitive files from the guest to the shared "env" folder.
        cp /home/unms/app/unms.conf /home/vagrant/env/unms.conf



        #echo "alias php='docker exec ucrm php'" >> ~/.bash_aliases
        #source ~/.bash_aliases

        #echo "alias php='sudo docker exec ucrm php'" >> /home/vagrant/.bash_aliases
        #chown vagrant:vagrant /home/vagrant/.bash_aliases

        #add-apt-repository -y ppa:ondrej/php
        #apt update -y
        #apt install -y \
        #    php7.4-cli \
        #    php7.4-xdebug

        #docker cp ucrm:/usr/local/bin/php /usr/bin/php
        #docker cp ucrm:/usr/local/etc/php /etc/php

    SHELL

end
