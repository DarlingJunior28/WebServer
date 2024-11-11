Vagrant.configure("2") do |config|
    # Configuración de la VM 1 (Servidor Web con Apache)
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/bionic64"
      web1.vm.network "private_network", type: "dhcp", ip: "192.168.33.10"
      web1.vm.provider "virtualbox" do |vb|
        vb.cpus = 1
        vb.memory = "512"
      end
      web1.vm.provision "shell", inline: <<-SHELL
        sudo apt update
        sudo apt install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
        sudo mkdir -p /var/www/html
        sudo echo "Servidor Web 1" > /var/www/html/index.html
      SHELL
      web1.vm.synced_folder ".", "/var/www/html"
    end
  
    # Configuración de la VM 2 (Servidor Web con Apache)
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/bionic64"
      web2.vm.network "private_network", type: "dhcp", ip: "192.168.33.11"
      web2.vm.provider "virtualbox" do |vb|
        vb.cpus = 1
        vb.memory = "512"
      end
      web2.vm.provision "shell", inline: <<-SHELL
        sudo apt update
        sudo apt install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
        sudo mkdir -p /var/www/html
        sudo echo "Servidor Web 2" > /var/www/html/index.html
      SHELL
      web2.vm.synced_folder ".", "/var/www/html"
    end
  
    # Configuración de la VM 3 (Servidor con Nginx como balanceador de carga)
    config.vm.define "loadbalancer" do |loadbalancer|
      loadbalancer.vm.box = "ubuntu/bionic64"
      loadbalancer.vm.network "private_network", type: "dhcp", ip: "192.168.33.12"
      loadbalancer.vm.provider "virtualbox" do |vb|
        vb.cpus = 1
        vb.memory = "512"
      end
      loadbalancer.vm.provision "shell", inline: <<-SHELL
        sudo apt update
        sudo apt install -y nginx
        sudo systemctl enable nginx
        sudo systemctl start nginx
        # Configurar Nginx como balanceador de carga
        sudo bash -c 'cat > /etc/nginx/sites-available/default <<EOF
  server {
      listen 80;
  
      location / {
          proxy_pass http://192.168.33.10;
          proxy_pass http://192.168.33.11;
      }
  }
  EOF'
        sudo systemctl restart nginx
      SHELL
    end
  end
  