# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

    BOX_IMAGE = "bento/ubuntu-22.04"

    config.vbguest.auto_update = true
    config.vbguest.no_remote = true
    config.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant", create: true
        
        # config monitoring server

        config.vm.define "server" do |subconfig|

            subconfig.vm.box = BOX_IMAGE

            subconfig.vm.network :private_network, ip: "10.0.2.11"

            subconfig.vm.provision "ansible_local" do |ansible|
              ansible.become = true
              ansible.playbook = "/vagrant/data/ansible-prometheus-grafana/playbook.yml"
              ansible.galaxy_command = "sudo ansible-galaxy install -r /vagrant/data/ansible-prometheus-grafana/requirements.yml"
            end

            subconfig.vm.provider "virtualbox" do |vb|
              vb.gui = true
              vb.memory = "2048"
              vb.cpus = 2
            end


            subconfig.vm.provision "shell", inline: <<-SHELL
              sudo apt-get update
              sudo /bin/sh -c 'if [ -d /home/vagrant/ansible-prometheus-grafana ]; then rm -Rf /home/vagrant/ansible-prometheus-grafana; fi'
              sudo ls -la /data
              sudo chown -R vagrant:vagrant ansible-prometheus-grafana/
              sudo apt-get install -y ubuntu-desktop
              sudo apt-get install -y gnome-core gdm3
              sudo systemctl set-default graphical.target
              sudo reboot
            SHELL

          end

    # config api server
  
    config.vm.define "node-server" do |subconfig|
        subconfig.vm.box = BOX_IMAGE
        subconfig.vm.hostname = "node-server"
        subconfig.vm.network :private_network, ip: "10.0.2.10"
        
        subconfig.vm.provision "shell", inline: <<-SHELL
          sudo apt update
          sudo curl -fsSL https://fnm.vercel.app/install | bash
          sudo source ~/.bashrc
          sudo fnm use --install-if-missing 22
          sudo apt install -y mariadb-server
          sudo systemctl start mariadb
          sudo systemctl enable mariadb
          sudo mysql_secure_installation <<EOF
Y
n
Y
Y
Y
EOF
          sudo mysql -u root -e "SET PASSWORD FOR 'root'@'localhost' = PASSWORD('');"
          sudo mysql -u root -e "FLUSH PRIVILEGES;"
          sudo git clone https://github.com/Manianise/api_nodejs.git
          sudo cd api-nodejs
          sudo npm install
          sudo npm run test
          sudo apt-get install -y ubuntu-desktop
          sudo apt-get install -y gnome-core gdm3
          sudo systemctl set-default graphical.target
          sudo reboot
        SHELL

        subconfig.vm.provider "virtualbox" do |vb|
            vb.gui = false
            vb.memory = "2048"
            vb.cpus = 2
        end

    end
  
  
    # Disable automatic box update checking. If you disable this, then
    # boxes will only be checked for updates when the user runs
    # `vagrant box outdated`. This is not recommended.
    # config.vm.box_check_update = false
  
    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine. In the example below,
    # accessing "localhost:8080" will access port 80 on the guest machine.
    # NOTE: This will enable public access to the opened port
    # config.vm.network "forwarded_port", guest: 80, host: 8080
  
    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine and only allow access
    # via 127.0.0.1 to disable public access
    # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  
    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
  
    # Create a public network, which generally matched to bridged network.
    # Bridged networks make the machine appear as another physical device on
    # your network.
    # config.vm.network "public_network"
  
    # Share an additional folder to the guest VM. The first argument is
    # the path on the host to the actual folder. The second argument is
    # the path on the guest to mount the folder. And the optional third
    # argument is a set of non-required options.
    # config.vm.synced_folder "../data", "/vagrant_data"
  
    # Disable the default share of the current code directory. Doing this
    # provides improved isolation between the vagrant box and your host
    # by making sure your Vagrantfile isn't accessible to the vagrant box.
    # If you use this you may want to enable additional shared subfolders as
    # shown above.
    # config.vm.synced_folder ".", "/vagrant", disabled: true
  
    # Provider-specific configuration so you can fine-tune various
    # backing providers for Vagrant. These expose provider-specific options.
    # Example for VirtualBox:
    #
    # config.vm.provider "virtualbox" do |vb|
    #   # Display the VirtualBox GUI when booting the machine
    #   vb.gui = true
    #
    #   # Customize the amount of memory on the VM:
    #   vb.memory = "1024"
    # end
    #
    # View the documentation for the provider you are using for more
    # information on available options.
  
    # Enable provisioning with a shell script. Additional provisioners such as
    # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
    # documentation for more information about their specific syntax and use.
    # config.vm.provision "shell", inline: <<-SHELL
    #   apt-get update
    #   apt-get install -y apache2
    # SHELL
  end
  