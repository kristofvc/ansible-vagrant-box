# Check to determine whether you're on a windows or linux/os-x host,
# later on we use this to launch ansible in the supported way (native or on your newly created box)
# source: https://stackoverflow.com/questions/2108727/which-in-ruby-checking-if-program-exists-in-path-from-ruby
def which(cmd)
    exts = ENV['PATHEXT'] ? ENV['PATHEXT'].split(';') : ['']
    ENV['PATH'].split(File::PATH_SEPARATOR).each do |path|
        exts.each { |ext|
            exe = File.join(path, "#{cmd}#{ext}")
            return exe if File.executable? exe
        }
    end
    return nil
end
Vagrant.configure("2") do |config|

    config.vm.provider :virtualbox do |v|
        v.name = "test-box"
        v.customize ["modifyvm", :id, "--memory", 1024]
    end

    config.vm.box = "ubuntu/trusty64"
    
    config.vm.box_url = "https://vagrantcloud.com/ubuntu/trusty64/version/1/provider/virtualbox.box"
    
    config.vm.network :private_network, ip: "192.168.33.10"

    config.ssh.forward_agent = true

    ###########################################################################################
    # Ansible provisioning (you need to have ansible installed or run the windows provisioning)
    # Windows provisioning installs ansible and your box and runs it there
    ###########################################################################################

    if which('ansible-playbook')
        config.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/playbook.yml"
            ansible.inventory_path = "ansible/inventories/dev"
            ansible.limit = 'all'
        end
    else
        config.vm.provision :shell, path: "ansible/windows.sh"
    end

    config.vm.synced_folder "./", "/vagrant", type: "nfs"
end
