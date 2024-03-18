BOX_IMAGE = "ubuntu/bionic64"
WORKER_COUNT = 2

Vagrant.configure("2") do |config|
  config.vm.box = BOX_IMAGE

  config.vm.define "kube-control-plane" do |node|
    node.vm.hostname = "kube-control-plane"
    node.vm.network :private_network, ip: "192.168.56.10"
    node.vm.provider :virtualbox do |vb|
      vb.name = "kube-control-plane"
      vb.memory = 2048
      vb.cpus = 2
    end
    node.vm.provision "shell", path: "common.sh", env: {"K8S_VERSION" => "1.26.1-1.1"}
  end

  (1..WORKER_COUNT).each do |i|
    config.vm.define "kube-worker-#{i}" do |node|
      node.vm.hostname = "kube-worker-#{i}"
      node.vm.network :private_network, ip: "192.168.56.#{i + 20}"
      node.vm.provider :virtualbox do |vb|
        vb.name = "kube-worker-#{i}"
        vb.memory = 1024
        vb.cpus = 2
      end
      node.vm.provision "shell", path: "common.sh", env: {"K8S_VERSION" => "1.26.1-1.1"}
    end
  end

  config.vm.provision "shell",
    run: "always",
    inline: "swapoff -a"
end
