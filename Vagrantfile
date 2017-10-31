# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  [8, 9].each do |java_version|

    config.vm.define "java#{java_version}" do |vh|
      vh.vm.box = "ubuntu/xenial64"

      # Because I use these VMs when testing play stuff
      vh.vm.network "forwarded_port", guest: 9000, host: (9090 + java_version)

      vh.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
      end

      vh.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y vim git git-core lynx
        # Add Java PPA
        add-apt-repository ppa:webupd8team/java -y
        apt-get update
        # Accept oracle license
        /bin/echo debconf shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
        /bin/echo debconf shared/accepted-oracle-license-v1-1 seen true | /usr/bin/debconf-set-selections
        # Install Java
        apt-get install oracle-java#{java_version}-installer oracle-java#{java_version}-set-default -y
        # Install sbt
        echo "deb https://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list
        apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823
        apt-get update
        apt-get install sbt
      SHELL
    end

  end

end
