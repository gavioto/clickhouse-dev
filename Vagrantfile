# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  config.vm.box = "bento/ubuntu-20.04"
  
  config.proxy.enabled = false

  config.vm.box_check_update = false

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true
  
    vb.cpus = 6
	
    # Customize the amount of memory on the VM:
    vb.memory = "8000"
	
	#Needed 
	#virtualbox.customize ['setextradata', :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate//vagrant", '1'
  end

  config.vm.provision "system-configuration", type: "shell", inline: <<-SHELL

	export DEBIAN_FRONTEND=noninteractive 
	
	apt -y update && apt -y install git gcc g++ gnome-session gnome-terminal cmake ninja-build clang
	
	snap install clion --classic
	
  SHELL
  
  config.vm.provision "checkout", type:"shell", inline: <<-SHELL

	cd /home/vagrant	
	rm -R ClickHouse
	git clone --recursive https://github.com/gavioto/ClickHouse.git
	cd ClickHouse

	#git submodule init
	#git submodule update

  SHELL

  config.vm.provision "compile", type:"shell", inline: <<-SHELL

    cd /home/vagrant/ClickHouse

	#ubuntu 20.04 doesn't  need this
	#export CC=gcc-10 CXX=g++-10

	mkdir build
	cd build
	cmake .. \
    -DCMAKE_C_COMPILER=/bin/clang-10 \
    -DCMAKE_CXX_COMPILER=/bin/clang++-10 \
    -DCMAKE_BUILD_TYPE=Debug \
    -DENABLE_CLICKHOUSE_ALL=OFF \
    -DENABLE_CLICKHOUSE_SERVER=ON \
    -DENABLE_CLICKHOUSE_CLIENT=ON \
    -DUSE_STATIC_LIBRARIES=OFF \
    -DSPLIT_SHARED_LIBRARIES=ON \
    -DENABLE_LIBRARIES=OFF \
    -DUSE_UNWIND=ON \
    -DENABLE_UTILS=OFF \
    -DENABLE_TESTS=OFF
	ninja -j 2 clickhouse-server clickhouse-client
    
  SHELL
  
 
end
