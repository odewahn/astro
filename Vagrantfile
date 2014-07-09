Vagrant.configure("2") do |config|

  # SSH forwarding: See https://help.github.com/articles/using-ssh-agent-forwarding
  config.ssh.forward_agent = true

  # update chef before running chef
  config.vm.provision :shell, :inline => "gem install chef --version 11.6.0.rc2 --no-rdoc --no-ri --conservative"

  #########################################################################
  # Virtualbox configuration - the default provider for running a local VM
  #########################################################################
  
  config.vm.provider :virtualbox do |vb, override|

    # The Virtualbox image
    override.vm.box = "precise64"
    override.vm.box_url = "http://files.vagrantup.com/precise64.box"

    # Port forwarding details
  
    # IPython Notebook
    override.vm.network :forwarded_port, host: 8888, guest: 8888
    override.vm.network :forwarded_port, host: 8000, guest: 8000

    config.vm.synced_folder ".", "/vagrant"
    
    # You can increase the default amount of memory used by your VM by
    # adjusting this value below (in MB) and reprovisioning.
    vb.customize ["modifyvm", :id, "--memory", "384"]
  end