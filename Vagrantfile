# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.


$script = <<SCRIPT
  sudo yum -y install vim
  sudo yum -y install mc
  sudo yum -y install net-tools
  sudo yum -y install java-devel
  sudo yum -y install zip unzip 
  sudo yum -y install mlocate

SCRIPT

Vagrant.configure("2") do |config|
   
  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.box = "sbeliakou/centos-7.4-x86_64-minimal"
    jenkins.vm.hostname = "jenkins" 
    jenkins.vm.network "private_network", ip: "172.16.1.20"
    jenkins.vm.network "private_network", ip: "172.16.1.21"
    jenkins.ssh.insert_key = false
    jenkins.vm.provider 'virtualbox' do |vb|
      vb.memory= "2048"
      vb.name = "jenkins"
    end
  jenkins.vm.provision 'shell', inline: $script
  jenkins.vm.provision 'shell', inline: <<-EOF
    mkdir /root/jenkins
    cd /root/jenkins
    yum -y install nginx
    yum -y install git
    sudo sed -i '/^        location/a \          proxy_pass http://127.0.0.1:8080;' /etc/nginx/nginx.conf
    systemctl restart nginx
    wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war
    java -jar jenkins.war
    
    
  EOF
  end
<<'COMM'
(1..1).each do |i|
  config.vm.define "node#{i}" do |node|
    node.vm.box = "sbeliakou/centos-7.4-x86_64-minimal"
    node.vm.hostname = "node#{i}"
    node.vm.network "private_network", ip: "172.16.1.#{i+20}"
    node.ssh.insert_key = false
    node.vm.provider 'virtualbox' do |vb|
      vb.memory= "1048"
      vb.name = "node#{i}"
    end

  node.vm.provision 'shell', inline: $script
  node.vm.provision 'shell', inline: <<-EOF

  EOF
  end 
end
COMM
end
