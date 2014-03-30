Playground for Vagrant, VirtualBox, Chef Solo, Librarian Chef
=============================================================

Getting Started
---------------

0. Install Vagrant from http://www.vagrantup.com/downloads.html

0. Install VirtualBox from https://www.virtualbox.org/wiki/Downloads

0. Create a Gemfile

        $ bundle init

0. Update Gemfile

        gem 'knife-solo'
        gem 'librarian-chef'

0. Run bundler

        $ bundle

0. Run knife solo to create chef structure

        $ knife solo init chef

0. Prepare Vagrant

        $ vagrant box add hashicorp/precise64
        $ vagrant init hashicorp/precise64

