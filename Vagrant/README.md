# Vagrant
## CLI
### vagrant ...
```sh
# This initialises the current directory to be a Vagrant environment by creating a Vagrantfile
vagrant init <boxe_name>
# This command creates ans configures guest machines according to the Vagrantfile
vagrat up
  # force the provisioners to run
  --provision
# This command will tell you the state of all active Vagrant environment on the system.
vagrant global-status
  # Prunes invalid entries from the list
  --prune
# This command shuts down the runninng machine
vagrant halt
# The equivalent of running "halt" followed by an up
vagrant reload
  # Force the provisioner to re-run
  --provision
# This command will stop the machine destroy all the resources that were created.
vagrant destroy <name|id>
  # A minimal version of the Vagrantfile will be created
  --minimal
  # This command will overwrite any existing Vagrantfile
  --force
  # Do not ask for confirmation before destroying
  -f 
```
### vagrant box
```sh
# Add the box with the given address.
add <Address>
# List all the boxes that are installed into Vagrant.
list
# Check whether or not the box you are using in you are using is outdated.
outdated
  # check for updates for all installed boxes and ignore cache interval
  --force
  # Check for updates for all installed boxes, not just for the current Vagrant environmment.
  --global
# Removes old versions of installed boxes.
prune
  # The specific provider type for the boxes to destroy.
  --provider <PROVIDER>
  # Only print the boxes that would be removed
  --dry-run
  # The specific box name to check for outdated versions.
  --name
  # Destroy without confirmation even when box is in user.
  --force
# Remove the box that match a given name.
remove <Name>
  # Version of version constraints of the boxes to remove.
  --box-version <VALUE>
  # Remove all available version of a box.
  --all
  # Forces removing the box even if an active Vagrant environment is using it.
  --force
  # The provider specific box to remove with the given name.
  --provider <VALUE>
# This command updates the box in the current Vagrant environment.
update
```
### Vagrant plugin
```sh
# Intall a plugin with the given name.
install <plugin_name>
# List all installed plugins
list
# Uninstall the plugin with the given name.
uninstall <plugin_name> <plugin_name>
# Update all installed plugins
update
```
## Vagrantfile
Every Vagrant file should start with
```ruby
Vagrant.configure("2") do |config|
```
### VM configs
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
# What we want to setup.
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
# Upload file or directory form the host machine to the guest machine.
config.vm.provision "file", source: "file", destination "file" -- 
```
### SSH configs
```ruby
# If set to true, Vagrant will automatically insert a keypair to use ssh, replacing Vagrant's default insecure key.
config.ssh.insert_key
# If true, agent forwarding over SSH connection is enabled.
config.ssh.forward_agent
```
### Network options
```ruby
# Create a forwarded port mapping.
"forwarded_port", guest: 80, host: 8080
# Create a private network, which allows host only access.
"private_network", ip: "192.168.33.10"
  # to disable Vagrant's auto-configure feature.
  auto_config: false 

```
### Defining Multi Machines
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
### Ansible
```ruby
config.vm.provision "ansible" do |ansible|
  ansible.limit = "all" -- allow access to all machines by ansible
  # Playbook location
  ansible.playbook = "playbook.yml" -- 
  # Allow/disable verbosity
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
    # When all the machines are up and ready.
    if machine_id == N
      machine.vm.provision :ansible do |ansible|
        ansible.limit = "all"
        ansible.playbook = "playbook.yml"      
      end    
    end  
  end
end
```
