vagrant-chefsolo-librarian
==========================

Playing around with Vagrant, chef-solo and librarian-chef

Steps
-----

0. Create a Gemfile

        $ bundle init

0. Update Gemfile

        gem 'knife-solo'
        gem 'librarian-chef'

0. Run bundler

        $ bundle

0. Run knife solo to create chef structure

        $ knife solo init chef


