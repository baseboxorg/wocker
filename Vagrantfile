# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.define "wocker"
  config.vm.box = "box-cutter/ubuntu1404-docker"
  
  config.ssh.username = "docker"
  config.ssh.password = "docker"
  
  if Vagrant.has_plugin?("vagrant-triggers") then
    config.trigger.after [:up, :resume] do
      info "Adjusting datetime after suspend and resume."
      run_remote "sudo sntp -4sSc pool.ntp.org; date"
    end
  end

  # Adjusting datetime before provisioning.
  config.vm.provision :shell, run: "always" do |sh|
    sh.inline = "sntp -4sSc pool.ntp.org; date"
  end

  if Vagrant.has_plugin?("vagrant-hostsupdater")
    config.hostsupdater.remove_on_suspend = true
  end

  config.vm.hostname = "wocker.dev"
  config.vm.network :private_network, ip: "10.0.23.16"
  config.ssh.insert_key = true
  
  config.vm.synced_folder "./data", "/home/docker/data", create: true

  config.vm.provision :shell do |s|
    s.path = 'provision.sh'
  end

end
