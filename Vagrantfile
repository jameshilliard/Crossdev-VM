#Must have newer than 4.3 virtualbox SVN 90468
Vagrant.require_plugin "vagrant-vbguest"
Vagrant.configure("2") do |config|
  # Here we download the ubuntu cloud image Config is currently set for the 32 bit image Uncomment for 64 bit box
  #config.vm.box = "saucy64"
  #config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/saucy/current/saucy-server-cloudimg-amd64-vagrant-disk1.box"
  # Comment these out if you want to use 64 bit
   config.vm.box = "saucy32"
   config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/saucy/current/saucy-server-cloudimg-i386-vagrant-disk1.box"

  # Here we configure out host-only network, this is sometimes buggy and may cause various network glitches this will break if not on SVN90468 or above virtualbox
   config.vm.network "private_network", ip: "192.168.56.10"

  # Here we change the default vagrant sync folder so that we don't get extra things from the repo in it that we don't want
  config.vm.synced_folder "./vagrantsync", "/vagrant"

  #Here we do a number of performance enhancements system compatability is unknown so I will comment most out by default
  config.vm.provider "virtualbox" do |vb|
    #uncommenting this option will attmpt to destroy unused interfaces using vagrant destroy unpredicatable affects
    #vb.destroy_unused_network_interfaces = true
	
    # Virtualbox Name Change to 64 bit version if that is what is being used
    #vb.customize ["modifyvm", :id, "--name", "Crossdev-VM", "--ostype", "Ubuntu_64"]
    vb.customize ["modifyvm", :id, "--name", "Crossdev-VM", "--ostype", "Ubuntu"]
	
    # Memory uncomment to turn on set to 4 gigs, make sure you have enough memory for whatever amount you choose
    #vb.customize ["modifyvm", :id, "--memory", "4092"]
	#CPU up to 4 cores PAE and IOAPIC support uncomment to turn on
	#vb.customize ["modifyvm", :id, "--ioapic", "on"]
	#vb.customize ["modifyvm", :id, "--cpus", "4"]
	#vb.customize ["modifyvm", :id, "--pae", "on"]
	
    # Chipset (Supposedly better CPU performance) Make sure your host supports this
    #vb.customize [ "modifyvm", :id, "--chipset", "ich9" ]
	
    # NIC 1 (Better TCP over NAT performance, at least on Windows) This probably won't break older systems but comment out if it does
	vb.customize ["modifyvm", :id,"--nictype1", "virtio", "--natsettings1", "9000,1024,1024,1024,1024"]  
	
    # NIC 2 (Host Only Access) This is just setting the interface type to virtio which is faster, should work as long as guest supports it
	vb.customize ["modifyvm", :id, "--nic2", "hostonly", "--nictype2", "virtio"] 

    # This is a hack to get the proper path for saving our data image to the current directory
      disk2_path = Dir.pwd() + "/" + "Crossdev-Data" + ".vmdk"
	  
    # This is just adding a second port to our SATA controller to accomodate our data drive
    vb.customize ["storagectl", :id, "--name", "SATAController", "--controller", "IntelAHCI", "--portcount", "2", "--hostiocache", "on"]
	
    # Here We do some SSD performance enhancements to our primary guest image disabled by default
    #vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "0", "--device", "0", "--nonrotational", "on"]
	
	# Here we generate an empty virtualbox image default 300gb this is variable though so it will only use the space needed
    vb.customize ["createhd", "--filename", disk2_path, "--size", 300*1024, "--format", "vmdk", "--variant", "Standard"]
	
	# This attaches our empty generated virtualbox image to our SATA controller
    vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "1", "--device", "0", "--type", "hdd", "--medium", disk2_path]
  end
  
  # This is an incomplete script, commenting out for now
  #config.vm.provision "puppet"
  
end
