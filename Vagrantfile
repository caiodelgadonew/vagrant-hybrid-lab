# -*- mode: ruby -*-
# vi: set ft=ruby  :

#########################################################################################################
#													#
# 	This Vagrantfile make an hybrid lab on virtualbox with Linux, Mac and Windows Machines		#
#													#
# 	Default Configuration requires 13GB of free RAM							#
# 	4GB of RAM its the minimal for Windows and MacOSx 						#
# 	Increase or decrease RAM configuration for any machine if necessary				#
#	Dont forget to create the entries on hosts files 						#
#		Linux   -> /etc/hosts 									#
#		Windows -> C:\Windows\system32\drivers\etc\hosts					#
#													#
#		10.11.11.10	server.caiodelgado.example						#
#		10.11.11.11	database.caiodelgado.example						#
#		10.11.11.20	ubuntu.caiodelgado.example						#
#		10.11.11.21	centos.caiodelgado.example						#
#		10.11.11.30	windows.caiodelgado.example						#
#		10.11.11.40	macosx.caiodelgado.example						#
#													#
#########################################################################################################


machines = {
  "server"   => {"memory" => "2048", "cpu" => "2", "ip" => "10", "image" => "debian/buster64"},
  "database" => {"memory" => "1024", "cpu" => "1", "ip" => "11", "image" => "debian/buster64"},
  "ubuntu"   => {"memory" => "1024", "cpu" => "1", "ip" => "20", "image" => "ubuntu/bionic64"},
  "centos"   => {"memory" => "1024", "cpu" => "1", "ip" => "21", "image" => "centos/7"},
  "windows"  => {"memory" => "4096", "cpu" => "2", "ip" => "30", "image" => "inclusivedesign/windows10-eval"},
  "macosx"   => {"memory" => "4096", "cpu" => "2", "ip" => "40", "image" => "yzgyyang/macOS-10.14"}
}

Vagrant.configure("2") do |config|

  config.vm.box_check_update = false
  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.box = "#{conf["image"]}"
      if "#{name}" != "windows"
        machine.vm.hostname = "#{name}.caiodelgado.example"
      end
      machine.vm.network "private_network", ip: "10.11.11.#{conf["ip"]}"
      if "#{name}" == "macosx" 
        machine.vm.synced_folder ".", "/vagrant",
          id: "core",
          :nfs => true,
          :mount_options => ['nolock,vers=3,udp,noatime,actimeo=1,resvport'],
          :export_options => ['async,insecure,no_subtree_check,no_acl,no_root_squash']
      end
      machine.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.memory = conf["memory"]
        vb.cpus = conf["cpu"]
        vb.customize ["modifyvm", :id, "--groups", "/hybrid-lab"]
        if "#{name}" == "macosx"
          vb.gui = true
          vb.customize ["modifyvm", :id, "--cpuid-set", "00000001", "000106e5", "00100800", "0098e3fd", "bfebfbff"]
          vb.customize ["setextradata", :id, "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct", "MacBookPro11,3"]
          vb.customize ["setextradata", :id, "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion", "1.0"]
          vb.customize ["setextradata", :id, "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct", "Iloveapple"]
          vb.customize ["setextradata", :id, "VBoxInternal/Devices/smc/0/Config/DeviceKey", "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"]
          vb.customize ["setextradata", :id, "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC", "1"]
          # Set Resolution 0,1,2,3,4,5 :: 640x480, 800x600, 1024x768, 1280x1024, 1440x900, 1920x1200
          vb.customize ["setextradata", :id, "VBoxInternal2/EfiGopMode", "4"]
        end
      end
      if "#{name}" == "windows"
        machine.vm.provision "shell", 
          inline: <<-SHELL
            netsh.exe int ip show addresses
            netsh.exe int ip set address "Ethernet 2" static 10.11.11.#{conf["ip"]} 255.255.255.0 10.11.11.1
          SHELL
      end
    end
  end
end
