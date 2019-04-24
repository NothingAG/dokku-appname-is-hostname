# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_NAME = ENV["BOX_NAME"] || "bento/ubuntu-18.04"
BOX_CPUS = ENV["BOX_CPUS"] || "1"
BOX_MEMORY = ENV["BOX_MEMORY"] || "1024"
DOKKU_DOMAIN = ENV["DOKKU_DOMAIN"] || "dokku.me"
DOKKU_IP = ENV["DOKKU_IP"] || "10.99.0.2"
FORWARDED_PORT = (ENV["FORWARDED_PORT"] || '8080').to_i
PUBLIC_KEY_PATH = "#{Dir.home}/.ssh/id_rsa.pub"

Vagrant::configure("2") do |config|
  config.ssh.forward_agent = true

  config.vm.box = BOX_NAME

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    # Ubuntu's Raring 64-bit cloud image is set to a 32-bit Ubuntu OS type by
    # default in Virtualbox and thus will not boot. Manually override that.
    vb.customize ["modifyvm", :id, "--ostype", "Ubuntu_64"]
    vb.customize ["modifyvm", :id, "--cpus", BOX_CPUS]
    vb.customize ["modifyvm", :id, "--memory", BOX_MEMORY]
  end

  config.vm.define "dokku", primary: true do |vm|
    vm.vm.synced_folder File.dirname(__FILE__), "/root/dokku"
    vm.vm.network :forwarded_port, guest: 80, host: FORWARDED_PORT
    vm.vm.hostname = "#{DOKKU_DOMAIN}"
    vm.vm.network :private_network, ip: DOKKU_IP

    # Use the same nameserver as the host machine in order to avoid the "too many redirects" problem.
    vm.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
      # enable NAT adapter cable https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=838999
      vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
    end

    vm.vm.provision :shell do |s|
      s.inline = <<-EOT
        wget https://raw.githubusercontent.com/dokku/dokku/v0.15.5/bootstrap.sh \
        && sudo DOKKU_TAG=v0.15.5 bash bootstrap.sh
 
        echo '"\e[5~": history-search-backward' > /root/.inputrc
        echo '"\e[6~": history-search-forward' >> /root/.inputrc
        echo 'set show-all-if-ambiguous on' >> /root/.inputrc
        echo 'set completion-ignore-case on' >> /root/.inputrc
      EOT
    end
  end
end
