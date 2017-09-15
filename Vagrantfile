# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "bento/ubuntu-16.04"

  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 8090, host: 8090

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2028"
  end
  
  config.vm.provision "shell", inline: <<-SHELL
    #############################################
    # Installing docker
    #############################################
    # See https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-using-the-repository
    sudo apt-get update
    sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo apt-key fingerprint 0EBFCD88
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt-get update
    sudo apt-get install docker-ce
    # Solving permission denied pb. 
    # See https://techoverflow.net/2017/03/01/solving-docker-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket/
    sudo usermod -a -G docker $USER 

    #############################################
    # Installing docker-compose
    #############################################
    # See https://docs.docker.com/compose/install/#install-using-pip
    sudo apt-get install -y python-pip
    pip install --upgrade pip
    sudo pip install docker-compose

    #############################################
    # Installing ruby
    #############################################
    # Following instructions in http://misheska.com/blog/2013/12/26/set-up-a-sane-ruby-cookbook-authoring-environment-for-chef/#linux
    sudo apt-get update
    sudo apt-get install -y build-essential git
    sudo apt-get install -y libxml2-dev libxslt-dev libssl-dev
    wget -O chruby-0.3.9.tar.gz https://github.com/postmodern/chruby/archive/v0.3.9.tar.gz
    tar -xzvf chruby-0.3.9.tar.gz
    cd chruby-0.3.9/
    sudo make install
    cd ..
    # rm chruby-0.3.8.tar.gz
    # rm -rf chruby-0.3.8
    git clone https://github.com/sstephenson/ruby-build.git
    cd ruby-build/
    sudo ./install.sh
    cd ..
    # rm -rf ruby-build
    echo 'source /usr/local/share/chruby/chruby.sh' >> /home/vagrant/.bashrc
    echo 'source /usr/local/share/chruby/auto.sh' >> /home/vagrant/.bashrc
    source /home/vagrant/.bashrc
    ruby-build 2.2.3 --install-dir /home/vagrant/.rubies/ruby-2.2.3
    source /home/vagrant/.bashrc
    chruby ruby-2.2.3
    echo 'chruby ruby-2.2.3' >> /home/vagrant.bashrc
    gem update --system
    gem install bundler

    #############################################
    # Installing PhantomJS
    #############################################
    # Following instructions in https://gist.github.com/julionc/7476620
    sudo apt-get update
    sudo apt-get install -y build-essential chrpath libssl-dev libxft-dev
    sudo apt-get install -y libfreetype6 libfreetype6-dev
    sudo apt-get install -y libfontconfig1 libfontconfig1-dev
    export PHANTOM_JS="phantomjs-2.1.1-linux-x86_64"
    wget https://bitbucket.org/ariya/phantomjs/downloads/$PHANTOM_JS.tar.bz2
    sudo tar xvjf $PHANTOM_JS.tar.bz2
    sudo mv $PHANTOM_JS /usr/local/share
    sudo ln -sf /usr/local/share/$PHANTOM_JS/bin/phantomjs /usr/local/bin

    # # Launching the tests
    # git clone https://github.com/cptactionhank/docker-atlassian-confluence.git
    # cd docker-atlassian-confluence/
    # bundler
    # bundle exec rake
  SHELL
end
