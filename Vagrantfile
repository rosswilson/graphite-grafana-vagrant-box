http_port = 1080
base_url = "http://localhost:#{http_port}"

Vagrant.configure("2") do |config|
	config.vm.box = "ubuntu/xenial64"

	config.vm.synced_folder ".", "/vagrant", disabled: true

	config.vm.network "forwarded_port", guest: 80, host: http_port
	config.vm.network "forwarded_port", guest: 2003, host: 2003

	config.vm.provider :virtualbox do |vb|
		vb.customize ["modifyvm", :id, "--memory", "2048"]
		vb.customize ["modifyvm", :id, "--cpus", "2"]
	end

	config.vm.provision :ansible do |ansible|
		# ansible.verbose = "vvv"
		ansible.playbook = "site.yml"
		ansible.extra_vars = {
			base_url: base_url
		}
	end
end
