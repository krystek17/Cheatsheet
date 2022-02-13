# Vagrantfile

Every Vagrant file should start with
```ruby
Vagrant.configure("2") do |config|
```
## VM configs
```ruby
config.vm.box -- operating system
config.vm.define -- name of the machine
config.vm.hostname -- hostname
config.vm.network -- How your host sees your box
config.vm.provider -- libvirt, virtualbox, etc
config.vm.synced_folder "host_folder" "vm_folder" -- How you access files from your computer
config.vm.provision -- what we want setup (shell, ansible)
```
```ruby
config.vm.provider "libvirt" do |v|
  v.memory = "512"
  v.memory = "1"
  end
```
```ruby
config.vm.provision "shell", path "script.sh" -- specify a path to a shell script on theh host machine.
config.vm.provision "file", source: "file", destination "file" -- upload file or directory form the host machine to the guest machine.
```
## SSH configs
```ruby
config.ssh.insert_key -- if set to true, Vagrant will automatically insert a keypair to use ssh, replacing Vagrant's default insecure key.
config.ssh.forward_agent -- if true, agent forwarding over SSH connection is enabled
```
## Network options
```ruby
"forwarded_port", guest: 80, host: 8080 --  create a forwarded port mapping
"private_network", ip: "192.168.33.10" -- create a private network, which allows host only access
  auto_config: false -- to disable Vagrant's auto-configure feature

```
## Ansible
```ruby
config.vm.provision "ansible" do |ansible|
  ansible.limit = "all" -- allow access to all machines by ansible
  ansible.playbook = "playbook.yml" -- run specific playbook
  ansible.verbose = "false" -- allow/disable verbosity
  ansible.compatibility_mode = "2.0" -- remove compatibility warning
  ansible.inventory_path = "inventory.ini"
  ansible.groups = {
      "group-1" => ["node-[1:2]"],
      "group-2" => ["node-3"],
    }

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
