IMAGE_NAME = "ubuntu/focal64"
CPUS = 2
HOSTNAME = "gitlab"
MEM = 4096

IP_BASE = "192.168.50."

VAGRANT_DISABLE_VBOXSYMLINKCREATE=1

$DEFAULT_NETWORK_INTERFACE = `ip route | awk '/^default/ {printf "%s", $5; exit 0}'`

Vagrant.configure("2") do |config|
    config.vm.box = IMAGE_NAME
    config.ssh.insert_key = false
    config.vm.synced_folder ".", "/vagrant", disabled: true

    config.vm.define HOSTNAME do |gitlab|
      gitlab.vm.hostname = HOSTNAME
      gitlab.vm.network "private_network", ip: "#{IP_BASE}#{20}"
      gitlab.vm.provider :virtualbox do |vb|
        vb.name = HOSTNAME
        vb.cpus = CPUS
        vb.memory = MEM
      end

#      gitlab.vm.provision "ansible" do |ansible|
#        ansible.playbook = "roles/gitlab.yaml"
#        ansible.extra_vars = {
#           username: "#{ENV['USERNAME'] || `whoami`}",
#            hostname: HOSTNAME
#        }
#      end

      $script = <<-SCRIPT
      echo "configured networks: \n"
      ip -br a |grep -i up|awk '{print "INTERFACE:",$1,"ADDRESS:",$3}'
      SCRIPT
      gitlab.vm.provision "shell" do |shell|
        shell.inline = $script
      end
    end
end