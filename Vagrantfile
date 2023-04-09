
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.network "public_network"
  config.vm.network "forwarded_port", guest: 80, host: 80
 
  # Para uso com Ip Fixo.
  # config.vm.network "private_network", ip: "192.168.33.10"
 
  # Para sincronizar as pastas locais com VMS
  config.vm.synced_folder "Docker-build", "/home/leobittes"
  config.vm.synced_folder "app", "/home/leobittes/app"
  # config.vm.synced_folder "site", "/home/leobittes/projeto-docker-compose/site"
  config.vm.provider "virtualbox" do |vb|
    vb.name = "docker+compose"
    vb.memory = "1024"
    end
  
  config.vm.provision "shell", inline: <<-SHELL
        
    # Adiciona Usuario com ROOT sem senha
    adduser leobittes --disabled-password --gecos ""
    usermod -aG sudo leobittes

    # Instala o Docker  
    apt-get update
    apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    apt-get update
    
    # instala o Docker-Compose
    curl -SL https://github.com/docker/compose/releases/download/v2.17.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    mkdir -m 0755 -p /etc/apt/keyrings
    chmod +x /usr/local/bin/docker-compose
    SHELL
end
