$script1 = <<SCRIPT
  sudo echo -n > /etc/hosts
  sudo echo "127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1 localhost localhost.localdomain localhost6 localhost6.localdomain6
172.20.20.11 server2" | sudo tee -a /etc/hosts
SCRIPT

$script2 = <<SCRIPT
  sudo echo -n > /etc/hosts
  sudo echo "127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1 localhost localhost.localdomain localhost6 localhost6.localdomain6
172.20.20.10 server1" | sudo tee -a /etc/hosts
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = "bertvv/centos72"  
  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = 2048
    vb.cpus = 2
  end

  config.vm.define "server1" do |server1|
      server1.vm.hostname = "server1"
      server1.vm.network "private_network", ip: "172.20.20.10"
      server1.vm.network "forwarded_port", guest: 8080, host: 18080
      server1.vm.provision "shell", inline: $script1
      server1.vm.provision "shell", inline: "sudo yum install git -y"
      server1.vm.provision "shell", inline: "sudo mkdir /home/vagrant/git1/"
      server1.vm.provision "shell", inline: "sudo git clone https://github.com/yurylab/training.git /home/vagrant/git1/ -b task1"
      server1.vm.provision "shell", inline: "tail /home/vagrant/git1/textfile.txt"
  end

  config.vm.define "server2" do |server2|
      server2.vm.hostname = "server2"
      server2.vm.network "private_network", ip: "172.20.20.11"
      server2.vm.network "forwarded_port", guest: 8090, host: 18090
      server2.vm.provision "shell", inline: $script2
  end
end
