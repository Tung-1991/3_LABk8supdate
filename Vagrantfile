Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true 
  config.hostmanager.manage_host = true

  #shell all
  common_shell_script = <<-SHELL
    #cau hinh tong
    sudo sh -c 'echo "export PS1=\"\\[\\e[92m\\]\\u@\\h:\\w\\\\$\\[\\e[m\\]\"" >> /home/vagrant/.bashrc'
    sudo timedatectl set-timezone 'Asia/Ho_Chi_Minh' 
    sudo systemctl stop apparmor && sudo systemctl disable apparmor
    sudo systemctl stop ufw && sudo systemctl disable ufw
    sudo apt-get install socat
    sudo apt install net-tools -y
    #them module vao kernel de phuc vu k8s hoat dong
    #tat swap, ipv6, bat ipv4 forward va cac tinh nang ve network
    sudo echo -e "br_netfilter\noverlay" | sudo tee /etc/modules-load.d/k8s.conf > /dev/null 
    sudo modprobe br_netfilter
    sudo echo -e "vm.swappiness=0\nnet.ipv4.ip_forward = 1\nnet.ipv6.conf.all.disable_ipv6 = 1\nnet.ipv6.conf.default.disable_ipv6 = 1\nnet.ipv6.conf.lo.disable_ipv6 = 1\nnet.bridge.bridge-nf-call-iptables = 1" | sudo tee -a /etc/sysctl.conf > /dev/null && sudo sysctl -p
    sudo sed -i 's/GRUB_CMDLINE_LINUX_DEFAULT.*/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash ipv6.disable=1"/' /etc/default/grub && sudo update-grub
    sudo swapoff -a && sudo sed -i '/\/swap.img/s/^/#/' /etc/fstab
    #them node vao hosts
    echo "192.168.41.10 control1" | sudo tee -a /etc/hosts
    echo "192.168.41.11 worker1" | sudo tee -a /etc/hosts
    echo "192.168.41.12 worker2" | sudo tee -a /etc/hosts
    echo "192.168.41.13 worker3" | sudo tee -a /etc/hosts
    #tao phan vung luu tru tuy nhien clone vagrant nay ko co free lvm nen can add o cung
    sudo mkdir /data
    #sudo lvcreate -L 20G -n data-lv ubuntu-vg
    #sudo mkfs.ext4 /dev/ubuntu-vg/data-lv  
    #sudo mount /dev/ubuntu-vg/data-lv /data
    #sudo sh -c 'echo "/dev/ubuntu-vg/data-lv /data ext4 defaults 0 0" >> /etc/fstab'
    #sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
    #sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
  SHELL
  config.vm.provision "shell", inline: common_shell_script

  #VM def
  vm_definitions = [
    { name: "worker1", ip: "192.168.41.11" },
    { name: "worker2", ip: "192.168.41.12" },
    { name: "worker3", ip: "192.168.41.13" },
    { name: "control1", ip: "192.168.41.10" }
  ]
  vm_definitions.each do |vm|
    config.vm.define vm[:name] do |node|
      node.vm.box = "gusztavvargadr/ubuntu-server-2404-lts"
      node.vm.hostname = vm[:name]
      node.vm.network "private_network", ip: vm[:ip]
      node.vm.provider "vmware_desktop" do |vb|
        vb.cpus = 2
        vb.memory = 4096
      end
      if vm[:name] != "control1"
        node.vm.provision "shell", inline: <<-SHELL, privileged: false
          sudo sysctl -p
        SHELL
      end
    end
  end

  #control1
  config.vm.define "control1" do |control1|
    control1.vm.box_download_insecure = true
    control1.vm.provision "shell", inline: <<-SHELL, privileged: false 
      ssh-keygen -t rsa -N '' -f ~/.ssh/id_rsa
      touch ~/.ssh/config
      echo "Host *
      StrictHostKeyChecking no" >> ~/.ssh/config
      sudo chmod 400 ~/.ssh/config 
      sudo apt install sshpass
      echo "vagrant" >> pass.txt
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@control1
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@worker1
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@worker2
      sshpass -f pass.txt ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@worker3
    SHELL
  end
end
