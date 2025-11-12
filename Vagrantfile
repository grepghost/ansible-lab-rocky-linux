Vagrant.configure("2") do |config|
  # --- Base configuration ---
  BOX_NAME = "rockylinux/9"           # Official Rocky Linux 9 box (ARM64 supported)
  VM_PROVIDER = :vmware_desktop       # Use :vmware_desktop for VMware Fusion

  config.vm.box = BOX_NAME
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # --- Network settings ---
  BASE_IP = "192.168.56."
  NODES = [
    { name: "control", ip: "#{BASE_IP}10" },
    { name: "node1",   ip: "#{BASE_IP}11" },
    { name: "node2",   ip: "#{BASE_IP}12" },
    { name: "node3",   ip: "#{BASE_IP}13" },
    { name: "node4",   ip: "#{BASE_IP}14" },
    { name: "node5",   ip: "#{BASE_IP}15" },
  ]

  # --- Provider defaults ---
  config.vm.provider VM_PROVIDER do |v|
    v.gui = false
    v.vmx["memsize"]  = "2048"
    v.vmx["numvcpus"] = "2"
  end

  # --- Node configuration ---
  NODES.each do |node|
    config.vm.define node[:name] do |m|
      m.vm.hostname = "#{node[:name]}.lab.local"
      m.vm.network "private_network", ip: node[:ip], vmware__vmnet: "vmnet2"

      m.vm.provision "shell", inline: <<-SHELL
        set -e
        sudo dnf -y makecache
        sudo dnf -y install bash-completion vim git curl tar

        # Update /etc/hosts for name resolution
        cat >/tmp/hosts.lab <<'EOF'
#{NODES.map { |n| "#{n[:ip]} #{n[:name]}.lab.local #{n[:name]}" }.join("\n")}
EOF
        sudo sed -i '/# LAB-BEGIN/,/# LAB-END/d' /etc/hosts
        echo "# LAB-BEGIN" | sudo tee -a /etc/hosts
        cat /tmp/hosts.lab | sudo tee -a /etc/hosts
        echo "# LAB-END" | sudo tee -a /etc/hosts

        # Create a 'student' user with passwordless sudo
        if ! id student &>/dev/null; then
          sudo useradd -m student
          echo "student ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/student
        fi
      SHELL
    end
  end
end

