$instances = 3
$instance_name_prefix = "kube"

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.ssh.forward_agent = true

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2048"]
  end

  (1..$instances).each do |i|
    config.vm.define vm_name = "%s-%02d" % [$instance_name_prefix, i] do |config|
      config.vm.hostname = vm_name

      ip = "192.168.33.#{10*i}"
      config.vm.network :private_network, ip: ip

      config.vm.provision :shell do |shell|
        shell.inline = "sed 's/127\.0\.0\.1/192\.168\.33\.#{10*i}/' -i /etc/hosts"
      end

      if i == 3
        config.vm.provision :ansible do |ansible|
          ansible.extra_vars = 'vars.yml'
          ansible.limit = 'all'
          ansible.playbook = 'playbook.yml'
          ansible.groups = {
          'all-nodes' => ['kube-01','kube-02','kube-03'],
          'kube-master' => ['kube-01'],
          'kube-nodes' => ['kube-02','kube-03'],
          }
          ansible.verbose = ENV['ANSIBLE_VERBOSE'] ||= "v"
          ansible.tags = ENV['ANSIBLE_TAGS'] ||= "all"
          ansible.raw_arguments = [
            '--become',
          ]
        end
      end
    end
  end
end
