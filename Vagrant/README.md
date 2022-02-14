# Vagrant
## CLI
```

```

Every Vagrant file should start with
```ruby
Vagrant.configure("2") do |config|
```
## VM configs
```ruby
# OS
config.vm.box
# Name of the machine
config.vm.define
# hostname
config.vm.hostname
# Network management
config.vm.network
# Hypervisor
config.vm.provider -- libvirt, virtualbox, etc.
# Accessing files from your computer.
config.vm.synced_folder "host_folder" "vm_folder" 
# what we want to setup.
config.vm.provision -- what we want setup (shell, ansible)
```
```ruby
config.vm.provider "libvirt" do |v|
  v.memory = "512"
  v.memory = "1"
 end
```
```ruby
# Specify a path to a shell script on the host machine.
config.vm.provision "shell", path "script.sh"
# upload file or directory form the host machine to the guest machine.
config.vm.provision "file", source: "file", destination "file" -- 
```
## SSH configs
```ruby
# if set to true, Vagrant will automatically insert a keypair to use ssh, replacing Vagrant's default insecure key.
config.ssh.insert_key
# if true, agent forwarding over SSH connection is enabled.
config.ssh.forward_agent
```
## Network options
```ruby
# create a forwarded port mapping.
"forwarded_port", guest: 80, host: 8080
# create a private network, which allows host only access.
"private_network", ip: "192.168.33.10"
  # to disable Vagrant's auto-configure feature.
  auto_config: false 

```
## Defining Multi Machines
```ruby
Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "echo Hello"

  config.vm.define "yin" do |yin|
    yin.vm.box = "generic/ubuntu2004"
  end

  config.vm.define "yang" do |yang|
    yang.vm.box = "generic/ubuntu2004"
  end
end
```
## Ansible
```ruby
config.vm.provision "ansible" do |ansible|
  ansible.limit = "all" -- allow access to all machines by ansible
  # Playbook location
  ansible.playbook = "playbook.yml" -- 
  # allow/disable verbosity
  ansible.verbose = "false"
  # Remove compatibility warning
  ansible.compatibility_mode = "2.0" 
  # Inventory location
  ansible.inventory_path = "inventory.ini"
  # Host groups
  ansible.groups = {
      "group-1" => ["node-[1:2]"],
      "group-2" => ["node-3"],
    }
```
Take advantage of ansible parallelism with the following:
```ruby
N = 3
(1..N).each do |machine_id|  
  config.vm.define "machine#{machine_id}" do |machine|
    machine.vm.hostname = "machine#{machine_id}"
    machine.vm.network "private_network", ip: "192.168.77.#{20+machine_id}"
    
    # Only execute once the Ansible provisioner,
    # when all the machines are up and ready.
    if machine_id == N
      machine.vm.provision :ansible do |ansible|
        ansible.limit = "all"
        ansible.playbook = "playbook.yml"      
      end    
    end  
  end
end
```
