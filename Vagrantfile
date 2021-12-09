# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
VAGRANT_VM_PROVIDER = "virtualbox"
#dist = "centos/7"
dist = "boxomatic/debian-11"

#provisioning = "yum -y install centos-release-ceph-nautilus; yum -y install ceph python-enum34"
provisioning = ""
servers=[
    {
      :hostname                      => "node3",
      :box                           => dist,
      :cpus                          => 1,
      :memory                        => 1024,
      :ip                            => "192.168.50.3",
      :provisioning                  => provisioning
    },
    {
      :hostname                      => "node4",
      :box                           => dist,
      :cpus                          => 1,
      :memory                        => 1024,
      :ip                            => "192.168.50.4",
      :provisioning                  => provisioning
    },
    {
      :hostname                      => "node5",
      :box                           => dist,
      :cpus                          => 1,
      :memory                        => 1024,
      :ip                            => "192.168.50.5",
      :provisioning                  => provisioning
    }
  ]

Vagrant.require_version ">= 2.2.0"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.provider "virtualbox" do |vm_config, override|
    vm_config.customize [
      "modifyvm", :id,
      "--natdnshostresolver1", "on",
      # some systems also need this:
      # "--natdnshostresolver2", "on"
    ]
  end
  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box                    = machine[:box]
      node.vm.hostname               = machine[:hostname]
      node.dns.tld                   = machine[:hostname]
      node.dns.patterns              = machine[:hostname]
      node.vm.disk :disk, size: "1GB", name: "extra_storage1"
      node.vm.disk :disk, size: "1GB", name: "extra_storage2"
      node.vm.disk :disk, size: "1GB", name: "extra_storage3"

      if servers.length > 1
        node.vm.post_up_message      = "Enter to this box using the commad VAGRANT_VAGRANTFILE=" + __FILE__ + " vagrant ssh " + machine[:hostname]
      else
        node.vm.post_up_message      = "Enter to this box using the commad VAGRANT_VAGRANTFILE=" + __FILE__ + " vagrant ssh"
      end

      if machine[:ip] != nil
        node.vm.network              :private_network, ip: machine[:ip]
      else
        node.vm.network              :private_network, type: "dhcp"
      end

      if machine[:provisioning] != nil
        node.vm.provision            "shell", inline: machine[:provisioning]
      end

      if VAGRANT_VM_PROVIDER === "virtualbox"
        node.vm.provider :virtualbox do |vbox|
          vbox.cpus                  = machine[:cpus]
          vbox.memory                = machine[:memory]
          vbox.name                  = machine[:hostname]
        end
      elsif VAGRANT_VM_PROVIDER === "libvirt"
        node.vm.provider :libvirt do |libvirt|
          libvirt.cpus               = machine[:cpus]
          libvirt.memory             = machine[:memory]
        end
      elsif VAGRANT_VM_PROVIDER === "docker"
        node.vm.provider :docker do |docker|
          docker.image               = machine[:box]
          docker.build_dir           = machine[:build_dir]
          docker.git_repo            = machine[:git_repo]
        end
      else
        puts "\x1b[0;30;41mUnsupported Vagrant VM Provider: " + VAGRANT_VM_PROVIDER + "\x1b[0m"
        exit 2
      end
    end
  end
end
