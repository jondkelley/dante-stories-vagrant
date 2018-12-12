# -*- mode: ruby -*-
# vi: set ft=ruby :

# According to https://github.com/michelson/dante-stories/issues/7
# You need
#   * postgres
#   * redis
#   * elasticsearch
#   * imagemagick
#   * ruby
# Use capistrano to deploy the application.
# See Also https://www.digitalocean.com/docs/one-clicks/ruby-on-rails/

Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/srv/local", mount_options: ["dmode=700,fmode=600"]
  config.vm.synced_folder "./special", "/srv/special", mount_options: ["dmode=755,fmode=755"]
  # run slave first
  config.vm.define "postgres" do |mysqlslave|
    postgres.vm.box = "ubuntu/precise64"
    postgres.vm.hostname = 'postgres.stories.localdomain'
    #mysqlslave.vm.synced_folder "./data/slave1", "/var/lib/mysql_vagrant" , id: "mysql",
    #owner: 108, group: 113,  # owner: "mysql", group: "mysql",
    #mount_options: ["dmode=775,fmode=664"]

    postgres.vm.network :private_network, ip: "192.168.100.11"

    postgres.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--name", "postgres.stories.localdomain"]
    end

    postgres.vm.provision :shell, path: "bootstrap-postgres.sh"
  end

  config.vm.define "redis" do |redis|
    redis.vm.box = "ubuntu/precise64"
    redis.vm.hostname = 'redis.stories.localdomain'

    redis.vm.network :private_network, ip: "192.168.100.12"

    redis.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "redis.stories.localdomain"]
    end

    redis.vm.provision :shell, path: "bootstrap-redis.sh"

  end

  config.vm.define "elasticsearch" do |elasticsearch|
    elasticsearch.vm.box = "ubuntu/precise64"
    elasticsearch.vm.hostname = 'elasticsearch.stories.localdomain'

    elasticsearch.vm.network :private_network, ip: "192.168.100.13"

    elasticsearch.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "elasticsearch.stories.localdomain"]
    end

    elasticsearch.vm.provision :shell, path: "bootstrap-elasticsearch.sh"

  end

  config.vm.define "rubyweb" do |rubyweb|
    rubyweb.vm.box = "ubuntu/precise64"
    rubyweb.vm.hostname = 'rubyweb.stories.localdomain'

    rubyweb.vm.network :private_network, ip: "192.168.100.14"

    rubyweb.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "rubyweb.stories.localdomain"]
    end

    rubyweb.vm.provision :shell, path: "bootstrap-rubyweb.sh"

  end
end
