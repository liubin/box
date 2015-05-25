# -*- mode: ruby -*-
# vi: set ft=ruby :

$shell = <<-SHELL
  sudo yum update -y

  # for ruby
  echo "installing Ruby 2.2.2"
  sudo yum install -y wget tar gcc g++ make automake autoconf curl-devel openssl-devel zlib-devel apr-devel apr-util-devel sqlite-devel tmux ImageMagick ImageMagick-devel git

  wget http://cache.ruby-lang.org/pub/ruby/2.2/ruby-2.2.2.tar.gz \
    && tar zxvf ruby-2.2.2.tar.gz \
    && cd ruby-2.2.2 \
    && autoconf \
    && ./configure --disable-install-doc --prefix=/usr \
    && make \
    && sudo make install \
    && cd ~ \
    && rm -rf ruby-2.2.2*


  gem sources -r https://rubygems.org/
  gem sources -a http://ruby.taobao.org/

  sudo gem sources -r https://rubygems.org/
  sudo gem sources -a http://ruby.taobao.org/

  # skip installing gem documentation
  sudo echo 'gem: --no-rdoc --no-ri' >> "/root/.gemrc"
  echo 'gem: --no-rdoc --no-ri' >> "$HOME/.gemrc"

  sudo gem install bundler

  echo "installing Docker"
  sudo yum install -y docker
  sudo chkconfig docker on

  # enable PostgreSQL server
  echo "installing PostgreSQL"
  sudo yum install -y http://yum.postgresql.org/9.4/redhat/rhel-7-x86_64/pgdg-centos94-9.4-1.noarch.rpm
  sudo yum install -y postgresql94-server postgresql94-contrib postgresql94-devel

  sudo /usr/pgsql-9.4/bin/postgresql94-setup initdb
  sudo chkconfig postgresql-9.4 on

  echo "installing nginx"
  sudo yum install -y http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
  sudo yum install -y nginx


SHELL


Vagrant.configure(2) do |config|

  config.vm.box = "chef/centos-7.0"

  config.vm.network :private_network, ip: "192.168.0.88"

  config.vm.network "forwarded_port", guest: 4567, host: 4567
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.network "forwarded_port", guest: 3306, host: 3306
  config.vm.network "forwarded_port", guest: 5432, host: 5432

  # config.vm.synced_folder "/Users/liubin/bitbucket", "/bitbucket"
  # config.vm.synced_folder "/Users/liubin/github", "/github"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision "shell", inline: $shell
end
