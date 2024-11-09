Vagrant.configure("2") do |config|
    # Configuración para el servidor Apache1
    config.vm.define "apache1" do |apache1|
      apache1.vm.box = "ubuntu/bionic64" # Puedes usar otras versiones si prefieres
      apache1.vm.network "private_network", ip: "192.168.33.10"
      apache1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache1.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        echo "Servidor Apache 1" | sudo tee /var/www/html/index.html
      SHELL
    end
  
    # Configuración para el servidor Apache2
    config.vm.define "apache2" do |apache2|
      apache2.vm.box = "ubuntu/bionic64"
      apache2.vm.network "private_network", ip: "192.168.33.11"
      apache2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        echo "Servidor Apache 2" | sudo tee /var/www/html/index.html
      SHELL
    end
  
    # Configuración para el servidor NGINX (balanceador de carga)
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.network "private_network", ip: "192.168.33.12"
      nginx.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      nginx.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y nginx
        sudo tee /etc/nginx/conf.d/load_balancer.conf > /dev/null <<EOL
        upstream apache_servers {
            server 192.168.33.10;
            server 192.168.33.11;
        }
        server {
            listen 80;
            location / {
                proxy_pass http://apache_servers;
            }
        }
        EOL
        sudo systemctl restart nginx
      SHELL
    end
  end  