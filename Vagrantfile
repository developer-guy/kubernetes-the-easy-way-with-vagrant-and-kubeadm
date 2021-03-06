IP_MASTER1 = "192.168.101.101"
IP_WORKER1 = "192.168.101.201"
IP_WORKER2 = "192.168.101.202"

POD_NW_CIDR = "10.244.0.0/16"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = false

  config.vm.define "cluster1-master1" do |node|

    node.vm.provider "virtualbox" do |vb|
        vb.name = "cluster1-master1"
        vb.memory = 2048
        vb.cpus = 2

       vb.customize [
            "modifyvm", :id, "--uartmode1", "file",
            File.join(Dir.pwd, "tmp/cluster1-master1.log")
        ]
    end

    node.vm.hostname = "cluster1-master1"
    node.vm.network :private_network, ip: IP_MASTER1

    node.vm.provision "set-hosts", :type => "shell", :path => "scripts/set-hosts.sh" do |s|
      s.args = ["enp0s8"]
    end

    node.vm.provision 'shell', inline: "cat /vagrant/certs/id_rsa.pub >> /root/.ssh/authorized_keys"
    node.vm.provision "update-dns", type: "shell", :path => "scripts/update-dns.sh"
    node.vm.provision "install-kube", type: "shell", :path => "scripts/install-kube.sh", env: {"NODE_IP" => "#{IP_MASTER1}"}
    node.vm.provision "install-master", type: "shell", :path => "scripts/install-master.sh", env: {"MASTER_IP" => "#{IP_MASTER1}", "POD_NW_CIDR" => "#{POD_NW_CIDR}"}
    node.vm.provision "configure-master1", type: "shell", :path => "scripts/configure-master1.sh"
  end

    config.vm.define "cluster1-worker1" do |node|

        node.vm.provider "virtualbox" do |vb|
            vb.name = "cluster1-worker1"
            vb.memory = 2048
            vb.cpus = 1

            vb.customize [
                "modifyvm", :id, "--uartmode1", "file",
                File.join(Dir.pwd, "tmp/cluster1-worker1.log")
            ]
        end

        node.vm.hostname = "cluster1-worker1"
        node.vm.network :private_network, ip: IP_WORKER1

        node.vm.provision "set-hosts", :type => "shell", :path => "scripts/set-hosts.sh" do |s|
          s.args = ["enp0s8"]
        end

        node.vm.provision 'shell', inline: "cat /vagrant/certs/id_rsa.pub >> /root/.ssh/authorized_keys"
        node.vm.provision "update-dns", type: "shell", :path => "scripts/update-dns.sh"
        node.vm.provision "install-kube", type: "shell", :path => "scripts/install-kube.sh", env: {"NODE_IP" => "#{IP_WORKER1}"}
        node.vm.provision "install-worker", type: "shell", :path => "scripts/install-worker.sh", env: {"MASTER_IP" => "#{IP_MASTER1}"}
    end

    config.vm.define "cluster1-worker2" do |node|

        node.vm.provider "virtualbox" do |vb|
            vb.name = "cluster1-worker2"
            vb.memory = 2048
            vb.cpus = 1

            vb.customize [
                "modifyvm", :id, "--uartmode1", "file",
                File.join(Dir.pwd, "tmp/cluster1-worker2.log")
            ]
        end

        node.vm.hostname = "cluster1-worker2"
        node.vm.network :private_network, ip: IP_WORKER2

        node.vm.provision "set-hosts", :type => "shell", :path => "scripts/set-hosts.sh" do |s|
          s.args = ["enp0s8"]
        end

        node.vm.provision 'shell', inline: "cat /vagrant/certs/id_rsa.pub >> /root/.ssh/authorized_keys"
        node.vm.provision "update-dns", type: "shell", :path => "scripts/update-dns.sh"
        node.vm.provision "install-kube", type: "shell", :path => "scripts/install-kube.sh", env: {"NODE_IP" => "#{IP_WORKER2}"}
        node.vm.provision "install-worker", type: "shell", :path => "scripts/install-worker.sh", env: {"MASTER_IP" => "#{IP_MASTER1}"}
    end
end
