Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  puts "Combien de VM voulez-vous créer ?"
  vm_count = STDIN.gets.to_i
  abort("Nombre de VM invalide") if vm_count <= 0

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 1
  end

  (1..vm_count).each do |i|
    config.vm.define "vm#{i}" do |vm|
      vm.vm.hostname = "vm#{i}"
      vm.disksize.size = "25GB"
      vm.vm.network "private_network", ip: "192.168.56.#{9 + i}"

      vm.vm.provision "shell", privileged: true, inline: <<-SHELL
        set -e

        # Clavier FR
        apt-get update
        apt-get install -y keyboard-configuration
        loadkeys fr

        # Création utilisateur
        useradd -m -s /bin/bash user#{i}
        echo "user#{i}:user#{i}" | chpasswd
        usermod -aG sudo user#{i}


        mkdir -p /home/user#{i}/.ssh
        chmod 700 /home/user#{i}/.ssh

        # Génération clé SSH
        ssh-keygen -t ed25519 -f /home/user#{i}/.ssh/id_vm#{i} -N ""

        # Autorisation SSH
        cat /home/user#{i}/.ssh/id_vm#{i}.pub >> /home/user#{i}/.ssh/authorized_keys
        chmod 600 /home/user#{i}/.ssh/authorized_keys
        chown -R user#{i}:user#{i} /home/user#{i}/.ssh

        # Copier clés vers l'hôte
        mkdir -p /vagrant/ssh_keys
        cp /home/user#{i}/.ssh/id_vm#{i} /vagrant/ssh_keys/vm#{i}
        cp /home/user#{i}/.ssh/id_vm#{i}.pub /vagrant/ssh_keys/vm#{i}.pub
        chmod 600 /vagrant/ssh_keys/vm#{i}

        # Sécurisation SSH
        sed -i 's/^#\\?PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config
        systemctl restart ssh
      SHELL
    end
  end
end
