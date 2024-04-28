Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.hostname = "xr-d"
  config.vm.provider "virtualbox" do |vb|
    vb.name = "xr-d"
    vb.memory = "4096"
    vb.cpus = 4
  end

  config.vm.network "public_network", use_dhcp_assigned_default_route: true
  config.vm.synced_folder ".", "/vagrant_data"
  config.vm.provision "shell", privileged: true, inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io
    sudo usermod -aG docker vagrant
  SHELL

  config.vm.provision "shell", run: "always", inline: "sudo reboot"
end
