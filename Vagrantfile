Vagrant.configure("2") do |config|
  # Imagem oficial do Ubuntu Server 22.04 LTS
  config.vm.box = "ubuntu/jammy64"

  # ----------------------------------------------------
  # VM 1: Servidor de Aplicação, Reverse Proxy e Monitorização
  # ----------------------------------------------------
  config.vm.define "vm1-app" do |app|
    app.vm.hostname = "vm1-app"
    app.vm.network "private_network", ip: "192.168.56.10"
    
    # Redirecionamento de porta para aceder ao Nginx a partir do PC anfitrião
    app.vm.network "forwarded_port", guest: 80, host: 8080
    app.vm.network "forwarded_port", guest: 443, host: 8443

    app.vm.provider "virtualbox" do |v|
      v.name = "TechSolutions-VM1-App"
      v.memory = 4096
      v.cpus = 2
    end
  end

  # ----------------------------------------------------
  # VM 2: Base de Dados PostgreSQL e Repositório Restic
  # ----------------------------------------------------
  config.vm.define "vm2-db" do |db|
    db.vm.hostname = "vm2-db"
    db.vm.network "private_network", ip: "192.168.56.20"

    db.vm.provider "virtualbox" do |v|
      v.name = "TechSolutions-VM2-DB"
      v.memory = 2048
      v.cpus = 1
    end
  end

  # Aprovisionamento automatizado com Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "ansible/site.yml"
    ansible.inventory_path = "ansible/inventory"
  end
end
