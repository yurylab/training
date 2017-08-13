$script1 = <<SCRIPT
  yum install -y tomcat tomcat-webapps tomcat-admin-webapps
  systemctl enable tomcat
  systemctl start tomcat
  systemctl stop firewalld
  mkdir /usr/share/tomcat/webapps/test/
SCRIPT

$script2 = <<SCRIPT
  yum install -y httpd
  systemctl enable httpd
  systemctl start httpd
  systemctl stop firewalld
  cp /vagrant/mod_jk.so /etc/httpd/modules/
  cp /vagrant/httpd.conf /etc/httpd/conf/
  cp /vagrant/workers.properties /etc/httpd/conf/
  systemctl reload httpd
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = "bertvv/centos72"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "1024"
    vb.cpus = 2
   end
   

  config.vm.define "server1" do |server1|
      server1.vm.hostname = "server1"
      server1.vm.network "private_network", ip: "10.10.10.11"
      server1.vm.provision "shell", inline: $script1
      server1.vm.provision "shell", inline: "touch /usr/share/tomcat/webapps/test/index.html"
      server1.vm.provision "shell", inline: "echo Tomcat-1 >> /usr/share/tomcat/webapps/test/index.html"
  end

  config.vm.define "server2" do |server2|
      server2.vm.hostname = "server2"
      server2.vm.network "private_network", ip: "10.10.10.12"
      server2.vm.provision "shell", inline: $script1
      server2.vm.provision "shell", inline: "touch /usr/share/tomcat/webapps/test/index.html"
      server2.vm.provision "shell", inline: "echo Tomcat-2 >> /usr/share/tomcat/webapps/test/index.html"
  end

  config.vm.define "server3" do |server3|
      server3.vm.hostname = "server3" 
      server3.vm.network "private_network", ip: "10.10.10.10"
      server3.vm.network "forwarded_port", guest: 80, host: 8090
      server3.vm.provision "shell", inline: $script2
  end

end
