Vagrant.configure("2") do |config|
    config.vm.box = "bento/ubuntu-22.04"
    config.vm.box_check_update = true
    config.vm.box_download_insecure=true 
    config.ssh.insert_key = false
    ram_db = 2048
    ram_app = 4096
    cpu_app = 2
  
      config.vm.define "Database" do |db|
        db.vm.hostname = "Database"
        db.vm.network "private_network", ip: "192.168.10.11"

        db.vm.provision "shell", inline: <<-SHELL
          apt-get update
          sudo apt install python3-pip -y
          pip install PyMySQL
          sudo apt-get install php-mysql -y
        SHELL

        db.vm.provider "virtualbox" do |v|
          v.memory = ram_db
          v.cpus = cpu_app
          v.name = "Database"
        end
      end

      config.vm.define "MediaWiki" do |app|
        app.vm.hostname = "MediaWiki"
        app.vm.network "private_network", ip: "192.168.10.12"

        app.vm.provision "shell", inline: <<-SHELL
          apt-get update
          sudo apt install python3-pip -y
          pip install PyMySQL
          sudo apt-get install php-mysql -y
          sudo apt-get install mariadb-client -y
        SHELL

        app.vm.provider "virtualbox" do |v|
          v.memory = ram_app
          v.cpus = cpu_app
          v.name = "MediaWiki"
        end
      end

    config.vm.define "master" do |master|
      master.vm.hostname = "master"
      master.vm.network "private_network", ip: "192.168.10.10"

      master.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y software-properties-common
      sudo apt-add-repository --yes --update ppa:ansible/ansible
      sudo apt update
      sudo apt install ansible -y 
      sudo apt install python3-pip -y
      sudo apt install python3-dev libpq-dev -y
      pip3 install PyMySQL 
      pip3 install psycopg2
      ssh-keygen -t rsa -b 4096 -N '' -f /home/vagrant/.ssh/id_rsa
      chown -R vagrant:vagrant /home/vagrant/.ssh
      chmod 600 /home/vagrant/.ssh/id_rsa
      cp /vagrant/mediawiki.conf.j2 /home/vagrant/mediawiki.conf.j2
      cp /vagrant/install_media_wiki.yml /home/vagrant/install_media_wiki.yml
      cp /vagrant/LocalSettings.php.j2 /home/vagrant/LocalSettings.php.j2
      SHELL

      master.vm.provider "virtualbox" do |v|
        v.memory = ram_db
        v.cpus = cpu_app
        v.name = "Master"
      end
    end
  end
  