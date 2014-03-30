Playground for Vagrant, VirtualBox, Chef Solo, Librarian Chef
=============================================================

Getting Started
---------------

Steps taken to rebuild this repo from scratch:

0. Install Vagrant from http://www.vagrantup.com/downloads.html

0. Add omnibus plugin to Vagrant so we can enforce a Chef version on all boxes

        $ vagrant plugin install vagrant-omnibus

0. Install VirtualBox from https://www.virtualbox.org/wiki/Downloads

0. Create a Gemfile

        $ bundle init

0. Update `Gemfile`

        gem 'knife-solo'
        gem 'librarian-chef'

0. Run bundler

        $ bundle

0. Run knife solo to create chef structure

        $ knife solo init chef

0. Prepare Vagrant

        $ vagrant box add hashicorp/precise64
        $ vagrant init hashicorp/precise64

0. Add some cookbooks to `chef/Cheffile`

        cookbook 'build-essential', '~> 1.4.4'
        cookbook 'nginx'

0. Install the cookbooks from the `chef` directory:

        $ librarian-chef install

0. Add a development node in `chef/nodes/development.json`

  ```json
  {
    "nginx": {
      "version": "1.4.7",
      "default_site_enabled": true,
      "source": {
        "modules": ["nginx::http_gzip_static_module", "nginx::http_ssl_module"]
      }
    },
    "run_list": [
      "recipe[nginx::source]"
    ]
  }
  ```

0. Configure omnibus in `Vagrantfile`

        config.omnibus.chef_version = "11.10.4"

0. Configure provision with `chef_solo` in `Vagrantfile`

        chef_dir = pathname(__file__).dirname.join('chef')
        development_node = json.parse(chef_dir.join('nodes', 'development.json').read)

        config.vm.provision :chef_solo do |chef|
          chef.cookbooks_path = ["chef/site-cookbooks", "chef/cookbooks"]
          chef.roles_path = "chef/roles"
          chef.data_bags_path = "chef/data_bags"
          chef.provisioning_path = "/tmp/vagrant-chef"

          chef.run_list = development_node.delete('run_list')
          chef.json = development_node
        end

0. Finally start vagrant

        $ vagrant up
        $ vagrant ssh

0. Check nginx is running from the box console

        $ sudo service nginx status


