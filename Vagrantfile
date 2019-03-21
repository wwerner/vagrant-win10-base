# -*- mode: ruby -*-
# vi: set ft=ruby :

# Install chocolatey & tools via choco
$script_install_choco = <<EOS
iwr -useb https://chocolatey.org/install.ps1 | iex
choco install -y vscode
choco install -y adb
choco install -y babun
choco install -y googlechrome
choco install -y javaruntime
EOS

# Install dependencies from the web
$script_install_apps = <<EOS
mkdir c:/Users/vagrant/install
iwr -useb https://www.flowvpn.com/FlowVPN-11.exe -outfile 'c:/Users/vagrant/install/FlowVPN-11.exe'
c:/Users/vagrant/install/FlowVPN-11.exe
EOS

# Configure Windows Firewall
$script_config_fw = <<EOS
# Just an example below as reminder how to use this
# netsh advfirewall firewall add rule name="MyApp" profile=domain,private,public protocol=any enable=yes DIR=In program="c:\Program Files\MyApp.exe" Action=Allow
EOS


Vagrant.configure("2") do |config|
  config.vm.box = "StefanScherer/windows_10"
  config.vm.hostname = "win10-vagrant-base"

  # perhaps you want to change this
  config.vm.network "public_network" 

  config.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true

  config.vm.communicator = "winrm"

  config.winrm.username = "vagrant"
  config.winrm.password = "vagrant"

  config.vm.guest = :windows
  config.windows.halt_timeout = 15

  config.vm.provider "virtualbox" do |vb|
    vb.name = "Win10 - Vagrant Base"
    vb.gui = true
    vb.memory = "4096"
    vb.cpus = "2"
    vb.customize ['modifyvm', :id, '--cableconnected1', 'on']
    vb.customize ["modifyvm", :id, "--vram", "256"]
    vb.customize ["modifyvm", :id, "--ostype", "Windows10"]
    vb.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
    vb.customize ["storageattach", :id, "--storagectl", "IDE Controller", "--port", "0", "--device", "1", "--type", "dvddrive", "--medium", "emptydrive"]

    # Need USB?
    # vb.customize ["modifyvm", :id, "--usb", "on"]
    # vb.customize ["modifyvm", :id, "--usbxhci", "on"]
  end

  config.vm.provision "shell", inline: $script_install_choco, privileged: true
  config.vm.provision "shell", inline: $script_install_apps, privileged: false
  config.vm.provision "shell", inline: $script_config_fw, privileged: true

end
