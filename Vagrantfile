# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

OS = "centos/7"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.define :invenio do |invenio|
    weko3.vm.hostname = "invenio.example.org"
    weko3.vm.box = OS
    weko3.vm.network :private_network, ip: "172.31.111.10"
    weko3.vm.provision "file", source: ".inveniorc", destination: ".inveniorc"
    weko3.vm.provision "shell", inline: "cp /vagrant/hosts /etc/hosts", privileged: true 
    weko3.vm.provision "shell", inline: "source .inveniorc && /vagrant/scripts/provision-web.sh", privileged: false
    weko3.vm.provision "shell", inline: "source .inveniorc && /vagrant/scripts/provision-postgresql.sh", privileged: false
    weko3.vm.provision "shell", inline: "source .inveniorc && /vagrant/scripts/provision-redis.sh", privileged: false
    weko3.vm.provision "shell", inline: "source .inveniorc && /vagrant/scripts/provision-elasticsearch.sh", privileged: false
    weko3.vm.provision "shell", inline: "source .inveniorc && /vagrant/scripts/provision-rabbitmq.sh", privileged: false
    weko3.vm.provision "shell", inline: "source .inveniorc && /vagrant/scripts/provision-worker.sh", privileged: false
    weko3.hostsupdater.aliases = ["web","postgresql", "redis","elasticsearch","rabbitmq","worker"]
    weko3.vm.provision "shell", privileged: false, inline: <<-EOF
      sudo /usr/share/elasticsearch/bin/plugin install analysis-kuromoji
      source ~/.inveniorc
      sudo systemctl restart elasticsearch
      sudo yum -y reinstall glibc glibc-common
      curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash
      echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.bashrc
      echo 'eval "$(pyenv init -)"' >> ~/.bashrc
      echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
      source "$HOME/.bashrc"
      pyenv install 3.5.4
      pyenv global 3.5.4
      pip install virtualenvwrapper
      echo 'if [ -f $HOME/.pyenv/versions/3.5.4/bin/virtualenvwrapper.sh ]; then' >> $HOME/.bashrc
      echo ' export WORKON_HOME=$HOME/.virtualenvs' >> $HOME/.bashrc
      echo ' source $HOME/.pyenv/versions/3.5.4/bin/virtualenvwrapper.sh' >> $HOME/.bashrc
      echo 'fi' >> $HOME/.bashrc
    EOF
   end
end
