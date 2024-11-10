Vagrant.configure("2") do |config|
    # Configuración común para todas las máquinas
    config.vm.box = "ubuntu/bionic64"  # Usamos una box de Ubuntu 18.04
    config.vm.network "private_network", type: "dhcp"  # Configurar red privada
    config.vm.provider "virtualbox" do |vb|
      vb.memory = "512"   # 512 MB de RAM
      vb.cpus = 1         # 1 core
    end
  
    # Máquina 1: Servidor Apache 1
    config.vm.define "apache1" do |apache1|
      apache1.vm.hostname = "apache1"
      apache1.vm.network "private_network", type: "static", ip: "192.168.33.10"
  
      # Directorio compartido con el archivo HTML
      apache1.vm.synced_folder ".", "/vagrant", type: "virtualbox"
  
      apache1.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
  
        # Copiar el archivo HTML a la raíz de Apache
        sudo cp /vagrant/index.html /var/www/html/index.html
      SHELL
    end
  
    # Máquina 2: Servidor Apache 2
    config.vm.define "apache2" do |apache2|
      apache2.vm.hostname = "apache2"
      apache2.vm.network "private_network", type: "static", ip: "192.168.33.11"
  
      # Directorio compartido con el archivo HTML
      apache2.vm.synced_folder ".", "/vagrant", type: "virtualbox"
  
      apache2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
  
        # Copiar el archivo HTML a la raíz de Apache
        sudo cp /vagrant/index.html /var/www/html/index.html
      SHELL
    end
  
    # Máquina 3: Servidor Nginx (Balanceador de carga)
    config.vm.define "nginx" do |nginx|
      nginx.vm.hostname = "nginx"
      nginx.vm.network "private_network", type: "static", ip: "192.168.33.12"
  
      nginx.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y nginx
        sudo systemctl enable nginx
        sudo systemctl start nginx
  
        # Configuración de balanceo de carga
        echo 'upstream backend {
          server 192.168.33.10;
          server 192.168.33.11;
        }
  
        server {
          listen 80;
  
          location / {
            proxy_pass http://backend;
          }
        }' | sudo tee /etc/nginx/sites-available/default
  
        # Reiniciar nginx para aplicar configuración
        sudo systemctl restart nginx
      SHELL
    end
  end
  