# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "public_network", ip: "192.168.1.88", bridge: "en0: Ethernet"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end
  config.vm.provision "shell", inline: <<-SHELL
apt-get update
apt-get install -y git X11-apps libxml2 libxml2-dev re2c libcurl3 libjpeg-dev icu-devtools libicu-dev libmcrypt-dev libtidy-dev libxslt1-dev libcurl4-openssl-dev libbz2-dev libpng12-dev g++ libreadline-dev autoconf ksh unzip default-jre libsqlite3-dev emacs24-nox

# 
su -l vagrant -c 'echo "# Program Language switch env" > ~/.bash_profile'

#
# Install phpenv & php-build 
#
su -l vagrant -c 'echo "# for PHP" >> ~/.bash_profile'
su -l vagrant -c 'git clone git://github.com/madumlao/phpenv.git ~/.phpenv'
su -l vagrant -c 'git clone git://github.com/CHH/php-build.git $HOME/.phpenv/plugins/php-build'
su -l vagrant -c 'echo export PATH=$HOME/.phpenv/bin:$PATH >> ~/.bash_profile'
su -l vagrant -c 'echo eval "$(phpenv init -)" >> ~/.bash_profile'

#
# Install ndenv & node-build
#
su -l vagrant -c 'echo "# for Node.js" >> ~/.bash_profile'
su -l vagrant -c 'git clone https://github.com/riywo/ndenv ~/.ndenv'
su -l vagrant -c 'git clone https://github.com/riywo/node-build.git ~/.ndenv/plugins/node-build'
su -l vagrant -c 'echo export PATH="$HOME/.ndenv/bin:$PATH" >> ~/.bash_profile'
su -l vagrant -c 'echo eval "$(ndenv init -)" >> ~/.bash_profile'

#
# Install rbenv & ruby-build
#
su -l vagrant -c 'echo "# for Ruby" >> ~/.bash_profile'
su -l vagrant -c 'git clone https://github.com/sstephenson/rbenv.git ~/.rbenv'
su -l vagrant -c 'git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build'
su -l vagrant -c 'echo export PATH="$HOME/.rbenv/bin:$PATH" >> ~/.bash_profile'
su -l vagrant -c 'echo eval "$(rbenv init -)" >> ~/.bash_profile'

#
# Install pyenv 
#
su -l vagrant -c 'echo "# for Python" >> ~/.bash_profile'
su -l vagrant -c 'git clone https://github.com/yyuu/pyenv.git ~/.pyenv'
su -l vagrant -c 'echo export PYENV_ROOT="$HOME/.pyenv" >> ~/.bash_profile'
su -l vagrant -c 'echo export PATH="$PYENV_ROOT/bin:$PATH" >> ~/.bash_profile'
su -l vagrant -c 'echo eval "$(pyenv init -)" >> ~/.bash_profile'

#
# for ALL
#
su -l vagrant -c 'exec $SHELL -l'

#
# Install Bluemix CLI
#
su -l vagrant -c 'mkdir download'
su -l vagrant -c 'cd download;curl -s -O https://public.dhe.ibm.com/cloud/bluemix/cli/bluemix-cli/Bluemix_CLI_0.5.5_amd64.tar.gz'
su -l vagrant -c 'cd download;tar xzvf Bluemix_CLI_0.5.5_amd64.tar.gz'
su -l vagrant -c 'sudo ./download/Bluemix_CLI/install_bluemix_cli'

#
# for Docker and Bluemix CLI IC plugin
#
apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
apt-key fingerprint 0EBFCD88
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs) stable"
apt-get update
apt-get install -y docker-ce
#
SHELL
end
