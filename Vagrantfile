# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"

  # Fix SSH forwarded port
  config.vm.network "forwarded_port", guest: 22, host:2222, id: "ssh", auto_correct: true

  # Access to Redis port
  config.vm.network "forwarded_port", guest: 6379, host:6379, id: "redis", auto_correct: true

  # Access to RabbitMQ UI
  config.vm.network "forwarded_port", guest: 15672, host:15672, id: "rabbitmqui", auto_correct: true

  # Access to MySQL DB
  config.vm.network "forwarded_port", guest: 3306, host:3306, id: "mysql", auto_correct: true

  # Run ansible playbook to deploy Redis, RabbitMQ and MySQL clusters
  config.vm.define "devopstest" do |devopstest|
    devopstest.vm.hostname = "devopstest"
    devopstest.vm.provision "ansible" do |ansible|
      ansible.inventory_path = "ansible/environments/devopstest/inventory"
      ansible.verbose = 'vvv'
      ansible.playbook = "ansible/devopstest.yml"
    end
    config.vm.provider "virtualbox" do |v|
      v.memory = 4096
    end
  end
end
