# Apple Silicon

## Virtual Machines

### Vagrant 2.2.18

- Vagrant: [release 2.2.18](https://releases.hashicorp.com/vagrant/2.2.18)
- Install: vagrant_2.2.18_x86_64.dmg

### VMware Fusion

- VMware Fusion Public Technology Preview 21H1 (for ARM): [Download](https://customerconnect.vmware.com/downloads/get-download?downloadGroup=FUS-PUBTP-2021H1)
- Vagrant: [FAQ](https://www.vagrantup.com/docs/providers/vmware/faq#q-how-do-i-use-the-vmware-fusion-tech-preview)
- Vagrant VMWare Utility: [Download](https://www.vagrantup.com/vmware/downloads)
  - It says x86_64 but it is fine.v
- gist: [Vagrant and VMWare Tech Preview on Apple M1 Pro](https://gist.github.com/sbailliez/f22db6434ac84eccb6d3c8833c85ad92)

```bash
sudo ln -s /Applications/VMware\ Fusion\ Tech\ Preview.app /Applications/VMware\ Fusion.app
vagrant plugin install vagrant-vmware-desktop
```

### Start

```bash
vagrant init generic/centos7
vagrant up
vagrant ssh
```

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos7"

  # config.vm.box_check_update = false

  config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  config.vm.network "private_network", ip: "192.168.56.26"
  
  # config.vm.network "public_network"

  config.vm.synced_folder "./share", "/share"

  config.vm.provider "vmware_desktop" do |vb|
    vb.ssh_info_public = true
    vb.cpus = 2
    vb.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
    yum install -y java-1.8.0-openjdk-headless
  SHELL
end
```
