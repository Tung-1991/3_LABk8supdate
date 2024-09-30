Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true 
  config.hostmanager.manage_host = true
  
  #config
  config.vm.provision "shell", inline: <<-SHELL
    #sudo apt update -y && sudo apt upgrade -y
    sudo systemctl stop apparmor && sudo systemctl disable apparmor
    sudo systemctl stop ufw && sudo systemctl disable ufw
    sudo apt install net-tools -y

    echo -e "vm.swappiness=0\nnet.ipv4.ip_forward = 1\nnet.ipv6.conf.all.disable_ipv6 = 1\nnet.ipv6.conf.default.disable_ipv6 = 1\nnet.ipv6.conf.lo.disable_ipv6 = 1" | sudo tee -a /etc/sysctl.conf > /dev/null && sudo sysctl -p
    sudo sed -i 's/GRUB_CMDLINE_LINUX_DEFAULT.*/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash ipv6.disable=1"/' /etc/default/grub && sudo update-grub

    echo "192.168.41.20 cicd" | sudo tee -a /etc/hosts
    echo "192.168.41.21 gitlab" | sudo tee -a /etc/hosts
    echo "192.168.41.22 rancher" | sudo tee -a /etc/hosts
    echo "192.168.41.23 control1" | sudo tee -a /etc/hosts
    echo "192.168.41.24 control2" | sudo tee -a /etc/hosts
    echo "192.168.41.25 control3" | sudo tee -a /etc/hosts
    echo "192.168.41.26 worker1" | sudo tee -a /etc/hosts
    echo "192.168.41.27 worker2" | sudo tee -a /etc/hosts
    echo "192.168.41.28 worker3" | sudo tee -a /etc/hosts
  SHELL

  #may control1
  config.vm.define "control1" do |control1|
    control1.vm.box_download_insecure = true
    config.vm.box = "bento/ubuntu-22.04"
    control1.vm.hostname = "control1"
    control1.vm.network "private_network", ip: "192.168.41.23"
    control1.vm.provider "vmware_desktop" do |vb|
     vb.cpus = 2
     vb.memory = 4096
    end
    control1.vm.provision "shell", inline: <<-SHELL, privileged: false
      sudo mkdir /data
      sudo lvcreate -L 20G -n data-lv ubuntu-vg
      sudo mkfs.ext4 /dev/ubuntu-vg/data-lv  
      sudo mount /dev/ubuntu-vg/data-lv /data
      sudo sh -c 'echo "/dev/ubuntu-vg/data-lv /data ext4 defaults 0 0" >> /etc/fstab'
      sudo lvextend -L +11G /dev/ubuntu-vg/ubuntu-lv
      sudo sysctl -p
    SHELL
  end
  
  #may control2
  config.vm.define "control2" do |control2|
    control2.vm.box_download_insecure = true
    config.vm.box = "bento/ubuntu-22.04"
    control2.vm.hostname = "control2"
    control2.vm.network "private_network", ip: "192.168.41.24"
    control2.vm.provider "vmware_desktop" do |vb|
     vb.cpus = 2
     vb.memory = 4096
    end
    control2.vm.provision "shell", inline: <<-SHELL, privileged: false
      sudo mkdir /data
      sudo lvcreate -L 20G -n data-lv ubuntu-vg
      sudo mkfs.ext4 /dev/ubuntu-vg/data-lv  
      sudo mount /dev/ubuntu-vg/data-lv /data
      sudo sh -c 'echo "/dev/ubuntu-vg/data-lv /data ext4 defaults 0 0" >> /etc/fstab'
      sudo lvextend -L +11G /dev/ubuntu-vg/ubuntu-lv
      sudo sysctl -p
    SHELL
  end
  
  #may control3
  config.vm.define "control3" do |control3|
    control3.vm.box_download_insecure = true
    config.vm.box = "bento/ubuntu-22.04"
    control3.vm.hostname = "control3"
    control3.vm.network "private_network", ip: "192.168.41.25"
    control3.vm.provider "vmware_desktop" do |vb|
     vb.cpus = 2
     vb.memory = 4096
    end
    control3.vm.provision "shell", inline: <<-SHELL, privileged: false
      sudo mkdir /data
      sudo lvcreate -L 20G -n data-lv ubuntu-vg
      sudo mkfs.ext4 /dev/ubuntu-vg/data-lv  
      sudo mount /dev/ubuntu-vg/data-lv /data
      sudo sh -c 'echo "/dev/ubuntu-vg/data-lv /data ext4 defaults 0 0" >> /etc/fstab'
      sudo lvextend -L +11G /dev/ubuntu-vg/ubuntu-lv
      sudo sysctl -p
    SHELL
  end
  
  #may worker1
  config.vm.define "worker1" do |worker1|
    worker1.vm.box_download_insecure = true
    config.vm.box = "bento/ubuntu-22.04"
    worker1.vm.hostname = "worker1"
    worker1.vm.network "private_network", ip: "192.168.41.26"
    worker1.vm.provider "vmware_desktop" do |vb|
     vb.cpus = 2
     vb.memory = 4096
    end
    worker1.vm.provision "shell", inline: <<-SHELL, privileged: false
      sudo mkdir /data
      sudo lvcreate -L 20G -n data-lv ubuntu-vg
      sudo mkfs.ext4 /dev/ubuntu-vg/data-lv  
      sudo mount /dev/ubuntu-vg/data-lv /data
      sudo sh -c 'echo "/dev/ubuntu-vg/data-lv /data ext4 defaults 0 0" >> /etc/fstab'
      sudo lvextend -L +11G /dev/ubuntu-vg/ubuntu-lv
      sudo sysctl -p
    SHELL
  end
  
  #may worker2
  config.vm.define "worker2" do |worker2|
    worker2.vm.box_download_insecure = true
    config.vm.box = "bento/ubuntu-22.04"
    worker2.vm.hostname = "worker2"
    worker2.vm.network "private_network", ip: "192.168.41.27"
    worker2.vm.provider "vmware_desktop" do |vb|
     vb.cpus = 2
     vb.memory = 4096
    end
    worker2.vm.provision "shell", inline: <<-SHELL, privileged: false
      sudo mkdir /data
      sudo lvcreate -L 20G -n data-lv ubuntu-vg
      sudo mkfs.ext4 /dev/ubuntu-vg/data-lv  
      sudo mount /dev/ubuntu-vg/data-lv /data
      sudo sh -c 'echo "/dev/ubuntu-vg/data-lv /data ext4 defaults 0 0" >> /etc/fstab'
      sudo lvextend -L +11G /dev/ubuntu-vg/ubuntu-lv
      sudo sysctl -p
    SHELL
  end
  
  #may worker3
  config.vm.define "worker3" do |worker3|
    worker3.vm.box_download_insecure = true
    config.vm.box = "bento/ubuntu-22.04"
    worker3.vm.hostname = "worker3"
    worker3.vm.network "private_network", ip: "192.168.41.28"
    worker3.vm.provider "vmware_desktop" do |vb|
     vb.cpus = 2
     vb.memory = 4096
    end
    worker3.vm.provision "shell", inline: <<-SHELL, privileged: false
      sudo mkdir /data
      sudo lvcreate -L 20G -n data-lv ubuntu-vg
      sudo mkfs.ext4 /dev/ubuntu-vg/data-lv  
      sudo mount /dev/ubuntu-vg/data-lv /data
      sudo sh -c 'echo "/dev/ubuntu-vg/data-lv /data ext4 defaults 0 0" >> /etc/fstab'
      sudo lvextend -L +11G /dev/ubuntu-vg/ubuntu-lv
      sudo sysctl -p
    SHELL
  end
  
  #may rancher
  config.vm.define "rancher" do |rancher|
    rancher.vm.box_download_insecure = true
    config.vm.box = "bento/ubuntu-22.04"
    rancher.vm.hostname = "rancher"
    rancher.vm.network "private_network", ip: "192.168.41.22"
    rancher.vm.provider "vmware_desktop" do |vb|
     vb.cpus = 2
     vb.memory = 4096
    end
    rancher.vm.provision "shell", inline: <<-SHELL, privileged: false
      sudo mkdir /data
      sudo lvcreate -L 15G -n data-lv ubuntu-vg
      sudo mkfs.ext4 /dev/ubuntu-vg/data-lv  
      sudo mount /dev/ubuntu-vg/data-lv /data
      sudo sh -c 'echo "/dev/ubuntu-vg/data-lv /data ext4 defaults 0 0" >> /etc/fstab'

      sudo mkdir /data2
      sudo lvcreate -L 16G -n data2-lv ubuntu-vg
      sudo mkfs.ext4 /dev/ubuntu-vg/data2-lv
      sudo mount /dev/ubuntu-vg/data2-lv /data2
      sudo sh -c 'echo "/dev/ubuntu-vg/data2-lv /data2 ext4 defaults 0 0" >> /etc/fstab'

      curl -fsSL https://get.docker.com/ | sh
      sudo usermod -aG docker vagrant
      sudo docker pull rancher/rancher:v2.9-head
      sudo sysctl -p
      sudo docker run --name rancher-server -d --restart=unless-stopped -p 6860:80 -p 6868:443 --privileged rancher/rancher:v2.9-head
    SHELL
  end
  
  #may Gitlab
  config.vm.define "gitlab" do |gitlab|
    gitlab.vm.box_download_insecure = true
    config.vm.box = "bento/ubuntu-22.04"
    gitlab.vm.hostname = "gitlab"
    gitlab.vm.network "private_network", ip: "192.168.41.21"
    gitlab.vm.provider "vmware_desktop" do |vb|
     vb.cpus = 2
     vb.memory = 4096
    end
    gitlab.vm.provision "shell", inline: <<-SHELL, privileged: false
      sudo mkdir /data
      sudo lvcreate -L 20G -n data-lv ubuntu-vg
      sudo mkfs.ext4 /dev/ubuntu-vg/data-lv  
      sudo mount /dev/ubuntu-vg/data-lv /data
      sudo sh -c 'echo "/dev/ubuntu-vg/data-lv /data ext4 defaults 0 0" >> /etc/fstab'
      sudo lvextend -L +11G /dev/ubuntu-vg/ubuntu-lv
      sudo sysctl -p
    SHELL
  end

  #may cicd
  config.vm.define "cicd" do |cicd|
    cicd.vm.box_download_insecure = true
    config.vm.box = "bento/ubuntu-22.04"
    cicd.vm.hostname = "cicd"
    cicd.vm.network "private_network", ip: "192.168.41.20"
    cicd.vm.provider "vmware_desktop" do |vb|
     vb.cpus = 1
     vb.memory = 1024
    end	
    cicd.vm.provision "shell", inline: <<-SHELL, privileged: false
      sudo timedatectl set-timezone 'Asia/Ho_Chi_Minh' 
      ssh-keygen -t rsa -N '' -f ~/.ssh/id_rsa
      touch ~/.ssh/config
      echo "Host *
      StrictHostKeyChecking no" >> ~/.ssh/config
      sudo chmod 400 ~/.ssh/config 
      
      sudo apt install sshpass
      echo "vagrant" >> pass.txt
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@control1
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@control2
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@control3
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@worker1
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@worker2
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@worker3
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@rancher
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@gitlab
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@cicd

      sudo apt install software-properties-common -y
      sudo add-apt-repository --yes --update ppa:ansible/ansible
      sudo apt install ansible -y
      sudo apt-get install python3-netaddr -y

      mkdir kubernetes_installation/
      cd kubernetes_installation && git clone https://github.com/kubernetes-sigs/kubespray.git && git checkout v2.24.1
      cd /home/vagrant/kubernetes_installation/kubespray && cp -rf inventory/sample inventory/nttung-cluster
      cd /home/vagrant/ && curl -fsSL https://get.docker.com/ | sh
      sudo usermod -aG docker vagrant
      sudo sysctl -p
    SHELL
  end
end


