# github-action-vagrant--vagrantfile-from-repository
This action is based on leleliu008's action:
https://github.com/leleliu008/github-actions-vagrant


This Github action will create virtual machine using Vagraint file in the root of your repository.
It is also exporting some bash variables into <repo_root>/shell.sh
and modifying line in Vagrant file starting with "config.vm.provision" so it can sync folders properly.