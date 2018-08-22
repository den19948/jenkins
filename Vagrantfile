# -*- mode: ruby -*-
# vi: set ft=ruby :
 

Vagrant.configure(2) do |config|
config.vm.define "mon" do |web_config|
 web_config.vm.box = "ubuntu/trusty64"

 web_config.vm.network "private_network", ip: "192.168.33.15"
  web_config.vm.provision "shell", inline: <<-SHELL
	sudo apt-get update
sudo apt install -y software-properties-common
sudo apt-add-repository -y ppa:webupd8team/java
sudo apt update
echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
sudo apt install -y oracle-java8-installer
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
echo deb http://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
sudo apt-get update
sudo apt-get install -y jenkins
   SHELL
end
config.vm.define :db do |db_config|
db_config.vm.box = "ubuntu/trusty64"
db_config.vm.network "private_network", ip: "192.168.33.25"
db_config.vm.provision "shell", inline: <<-SHELL
sudo apt-get update
sudo apt-get install -y apache2 git libapache2-mod-fastcgi apache2-mpm-worker
	sudo service apache2 restart
	sudo add-apt-repository ppa:ondrej/php
	sudo apt-get update
	sudo apt-get install -y python-software-properties software-properties-common
	sudo apt-get install -y  php5-mysql php5-curl php5-json php5-cgi  php5 libapache2-mod-php5
	sudo service apache2 restart 
SHELL
  end
config.vm.define :slave do |sl_config|
sl_config.vm.box = "ubuntu/trusty64"
sl_config.vm.network "private_network", ip: "192.168.33.35"
sl_config.vm.provision "shell", inline: <<-SHELL
sudo apt-get update
udo apt-get update
# Setting MySQL (username: root) ~ (password: password)
sudo debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password password password'
sudo debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password_again password password'
# Installing packages
apt-get install -y mysql-server mysql-client
# Setup database
#mysql -uroot -ppassword -e "CREATE DATABASE IF NOT EXISTS $APP_DATABASE_NAME;"
mysql -uroot -ppassword -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password';"
mysql -uroot -ppassword -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY 'password';"
mysql -uroot -ppassword -e "CREATE DATABASE wpdb;"
mysql -uroot -ppassword -e "GRANT ALL PRIVILEGES ON wpdb.* TO wpuser IDENTIFIED BY 'q1w2e3r4';"
sudo sed -i 's/bind-address/#bind-address/g' /etc/mysql/my.cnf 
sudo service mysql restart
SHELL
  end
end
