# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  # config.vm.box = "phusion/ubuntu-14.04-amd64"
  # config.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box"
  # config.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vmwarefusion.box"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network :forwarded_port, guest: 3010, host: 3010
  # config.vm.network :forwarded_port, guest: 4243, host: 4243

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.private_key_path = "~/.ssh/id_rsa"
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  # config.vm.synced_folder "./console", "/console"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

  # config.vm.provider :vmware_fusion do |f, override|
  #   override.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vmwarefusion.box"
  # end

  config.vm.define "default" do |console1|

    console1.vm.box = "phusion/ubuntu-14.04-amd64"
    console1.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vmwarefusion.box"

    console1.vm.network :forwarded_port, guest: 3010, host: 3010
    console1.vm.network :forwarded_port, guest: 4243, host: 4243

    console1.ssh.forward_agent = true

    console1.vm.synced_folder "./fresh_console", "/fresh_console"

    if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/default/*/id").empty?
      # Install Docker
      pkg_cmd = "wget -q -O - https://get.docker.io/gpg | apt-key add -;" \
        "echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list;" \
        "apt-get update -qq; apt-get install -q -y --force-yes lxc-docker; " \
        "apt-get install -q -y --force-yes build-essential zlib1g-dev curl git-core sqlite3 libsqlite3-dev;" \
        "apt-get install -q -y --force-yes libopenssl-ruby1.9.1 libssl-dev zlib1g-dev libreadline6 libreadline6-dev;" \
        "apt-get install -q -y --force-yes ruby1.9.1-dev libpq-dev libmysqlclient-dev;" \
        "apt-get install -q -y --force-yes sqlite3 libsqlite3-dev libyaml-dev libxml2-dev libxslt1-dev;"

        # "docker build -t rails ."
      # Add vagrant user to the docker group
      pkg_cmd << "usermod -a -G docker vagrant; "
      console1.vm.provision :shell, :inline => pkg_cmd
    end

    console1.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

  end

  config.vm.define "console2" do |console2|

    console2.vm.box = "phusion-open-ubuntu-14.04-amd64"
    console2.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box"

    console2.vm.network :forwarded_port, guest: 3020, host: 3020
    console2.vm.network :forwarded_port, guest: 5243, host: 5243
    console2.vm.network :forwarded_port, :host => 8005, :guest => 8005

    console2.ssh.forward_agent = true

    # console2.vm.synced_folder "./console_test", "/console"
    # console2.vm.synced_folder "./docmin", "/docker"

    if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/console2/*/id").empty?
      # Install Docker
      pkg_cmd = <<-CMD
        wget -q -O - https://get.docker.io/gpg | apt-key add -;
        echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list;
        apt-get update -qq; apt-get install -q -y --force-yes lxc-docker;
        apt-get install -q -y --force-yes build-essential zlib1g-dev curl git-core sqlite3 libsqlite3-dev;
        apt-get install -q -y --force-yes libopenssl-ruby1.9.1 libssl-dev zlib1g-dev libreadline6 libreadline6-dev;
        apt-get install -q -y --force-yes libpq-dev libmysqlclient-dev;
        apt-get install -q -y --force-yes sqlite3 libsqlite3-dev libyaml-dev libxml2-dev libxslt1-dev;
        docker run -name internal_registry -d -p 5000:5000 samalba/docker-registry
        echo "127.0.0.1      internal_registry" >> /etc/hosts
      CMD
      pkg_cmd << "usermod -a -G docker vagrant; "
      console2.vm.provision :shell, :inline => pkg_cmd
    end

    console2.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

  end

  config.vm.define "console3" do |console3|

    console3.vm.box = "phusion-open-ubuntu-14.04-amd64"
    console3.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box"

    console3.vm.network :forwarded_port, guest: 3030, host: 3030
    console3.vm.network :forwarded_port, guest: 5253, host: 5253

    console3.ssh.forward_agent = true

    console3.vm.synced_folder "./console_test", "/console"

    if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/console3/*/id").empty?

      pkg_cmd = <<-CMD
        wget -q -O - https://get.docker.io/gpg | apt-key add -;
        echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list;
        apt-get update -qq; apt-get install -q -y --force-yes lxc-docker;
        apt-get install -q -y --force-yes build-essential zlib1g-dev curl git-core sqlite3 libsqlite3-dev;
        apt-get install -q -y --force-yes libopenssl-ruby1.9.1 libssl-dev zlib1g-dev libreadline6 libreadline6-dev;
        apt-get install -q -y --force-yes libpq-dev libmysqlclient-dev;
        apt-get install -q -y --force-yes sqlite3 libsqlite3-dev libyaml-dev libxml2-dev libxslt1-dev;
        git clone git://github.com/sstephenson/rbenv.git /home/vagrant/.rbenv;
        echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> /home/vagrant/.bash_profile;
        echo 'eval "$(rbenv init -)"' >> /home/vagrant/.bash_profile;
        chown -R vagrant /home/vagrant/.rbenv;
        source .bash_profile;
        git clone git://github.com/sstephenson/ruby-build.git;
        sudo ruby-build/install.sh;
        chown -R vagrant /home/vagrant/ruby-build;
        /home/vagrant/.rbenv/bin/rbenv install 2.2.0;
        wget https://download.libsodium.org/libsodium/releases/libsodium-1.0.0.tar.gz;
        tar xzf libsodium-1.0.0.tar.gz;
        cd libsodium-1.0.0;
        ./configure;
        make && make check && sudo make install;
        sudo ldconfig;
        cd /console;
        gem install bundler;
        gem install rubygems-update
        update_rubygems
        gem update --system
        /home/vagrant/.rbenv/bin/rbenv rehash;
      CMD
      pkg_cmd << "usermod -a -G docker vagrant; "
      console3.vm.provision :shell, :inline => pkg_cmd
    end

    console3.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

  end

  config.vm.define "console_staging_box" do |console_staging_box|

    console_staging_box.vm.box = "phusion/ubuntu-14.04-amd64"
    console_staging_box.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box"

    console_staging_box.ssh.forward_agent = true

    console_staging_box.vm.network "private_network", ip: "192.168.1.1"

    if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/console_staging_box/*/id").empty?
      # Install Docker
      pkg_cmd = "wget -q -O - https://get.docker.io/gpg | apt-key add -;" \
        "echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list;" \
        "apt-get update -qq; apt-get install -q -y --force-yes lxc-docker; " \
        "apt-get install -q -y --force-yes build-essential zlib1g-dev curl git-core sqlite3 libsqlite3-dev;" \
        "apt-get install -q -y --force-yes libopenssl-ruby1.9.1 libssl-dev zlib1g-dev;" \
        "apt-get install -q -y --force-yes libpq-dev libmysqlclient-dev;" \
        "apt-get install -q -y --force-yes sqlite3 libsqlite3-dev libyaml-dev libxml2-dev libxslt1-dev;"# \
        # "git clone git://github.com/sstephenson/rbenv.git ~/.rbenv;" \
        # "echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile;" \
        # "echo 'eval "$(rbenv init -)"' >> ~/.bash_profile;" \
        # "source .bash_profile;" \
        # "git clone git://github.com/sstephenson/ruby-build.git;" \
        # "sudo ruby-build/install.sh;" \
        # "rbenv install 2.0.0-p481"
        # "wget https://download.libsodium.org/libsodium/releases/libsodium-0.4.5.tar.gz;" \
        # "tar xzf libsodium-0.4.5.tar.gz;" \
        # "cd libsodium-0.4.5;" \
        # "./configure;" \
        # "make && make check && sudo make install;" \
        # "sudo ldconfig;" \
        # "docker build -t rails ."
      # Add vagrant user to the docker group
      pkg_cmd << "usermod -a -G docker vagrant; "
      console_staging_box.vm.provision :shell, :inline => pkg_cmd
    end

    console_staging_box.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

  end

  config.vm.define "pixelserver" do |console4|

    console4.vm.box = "phusion/ubuntu-14.04-amd64"
    console4.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vmwarefusion.box"

    # console4.vm.network :forwarded_port, guest: 3040, host: 3040
    console4.vm.network :forwarded_port, guest: 9187, host: 9187

    console4.ssh.forward_agent = true

    console4.vm.synced_folder "~/apps/pixelserver", "/pixelserver"
    console4.vm.synced_folder "~/.gvm/pkgsets/go1.3/global", "/pixelserver/go"
    console4.vm.synced_folder "~/apps/pixel-ping", "/pixelserver/pixel-ping"

    if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/pixelserver/*/id").empty?
      # Install Docker
      pkg_cmd = "wget -q -O - https://get.docker.io/gpg | apt-key add -;" \
        "echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list;" \
        "apt-get update -qq; apt-get install -q -y --force-yes lxc-docker; " \
        "apt-get install -q -y --force-yes build-essential zlib1g-dev curl git-core sqlite3 libsqlite3-dev;" \
        "apt-get install -q -y --force-yes libopenssl-ruby1.9.1 libssl-dev zlib1g-dev libreadline6 libreadline6-dev;" \
        "apt-get install -q -y --force-yes ruby1.9.1-dev libpq-dev libmysqlclient-dev;" \
        "apt-get install -q -y --force-yes sqlite3 libsqlite3-dev libyaml-dev libxml2-dev libxslt1-dev;"

        # "docker build -t rails ."
      # Add vagrant user to the docker group
      pkg_cmd << "usermod -a -G docker vagrant; "
      console4.vm.provision :shell, :inline => pkg_cmd
    end

    console4.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

  end

  config.vm.define "web" do |web|
    web.vm.box = "chef/centos-6.5"

    web.ssh.forward_agent = true
    # web.vm.synced_folder "./console", "/console/current"

    web.vm.provider :virtualbox do |vb|
      vb.name = "Ansible web"
      # vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      # vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

    web.vm.network :forwarded_port, guest: 3010, host: 3010
    web.vm.network "private_network", ip: "192.168.10.11"

    # web.vm.provision "ansible" do |ansible|
    #   ansible.playbook = "ansiblesite/console_playbook.yml"
    #   ansible.inventory_path = "ansiblesite/vagrant"
    #   # ansible.verbose = 'vvv'
    # end
  end

  config.vm.define "worker" do |worker|
    worker.vm.box = "chef/centos-6.5"

    worker.ssh.forward_agent = true
    # worker.vm.synced_folder "./console", "/console/current"

    worker.vm.provider :virtualbox do |vb|
      vb.name = "Ansible worker"
    end

    worker.vm.network :forwarded_port, guest: 4243, host: 4243
    worker.vm.network "private_network", ip: "192.168.10.12"
  end

  config.vm.define "tableau" do |tableau|
    tableau.vm.box = "ubuntu/trusty64"

    tableau.ssh.forward_agent = true
    # tableau.vm.synced_folder "./console", "/console"

    tableau.vm.provider :virtualbox do |vb|
      vb.name = "Ansible tableau"
    end

    tableau.vm.network "private_network", ip: "192.168.10.13"
  end

  # config.vm.define "phusion" do |v|
  #   v.vm.provider "docker" do |d|
  #     d.cmd =
  #     d.image = "phusion/baseimage"
  #     d.has_ssh = true
  #   end

  #   v.ssh.username = "root"
  #   v.ssh.private_key_path = "~/.ssh/id_rsa"

  #   v.vm.provision "shell", inline: "echo Hello"
  # end

  # config.vm.provision "docker" do |d|
  #   d.run "rails", args: "-v /console:/console -p 3000:3000"
  # end
end
