#Vagrantfile

Every Vagrant file should start with
```ruby
Vagrant.configure("2") do |config|
```
## Main config
```ruby
config.vm.box => operating system
config.vm.define => name of the machine
config.vm.hostname => hostname
config.vm.network => How your host sees your box
config.vm.provider => libvirt, virtualbox, etc
config.vm.synced_folder => How you access files from your computer
config.vm.provision => what we want setup
```
## Network
```ruby
```
## Ansible
```ruby

```
