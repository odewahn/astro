# Notes for vagrant

Vagrant is a tool that makes it easy to create and use virtual machines. To use it, you need to install:
 
* Install [virtualbox](https://www.virtualbox.org/)
* Install [vagrant](http://www.vagrantup.com/)
* Install [git](http://git-scm.com/)  (well, ok, you don't need this for vagrant, but you should have it, anyway.)

Once you have this all done, rehash and then try running this on the command line:

```
vagrant --version
```

If this works, you're good.

## Using Vagrant

Every vagrant VM needs a file called *Vagrantfile* that defines the box.  The simplest one is:

```
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
```

When you start a new project you can use this as an example.  The main thing this does is:

* map the current directory on the host machine (".") to the "/vagrant" directory on the guest

## Helpful commands

Here are a few commands that will come in handy.

```
vagrant up
```

This will start the box for the first time.

```
vagrant ssh
```

This will let you log into the box

```
vagrant halt
```

This shuts the box down.

