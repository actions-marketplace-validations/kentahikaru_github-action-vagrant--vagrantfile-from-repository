# Description
This action is based on leleliu008's action:
https://github.com/leleliu008/github-actions-vagrant


This Github action will create virtual machine using Vagrant file in the root of your repository.
It is also:
* exports some bash variables into <repo_root>/shell.sh
* modifies line in Vagrant file starting with "config.vm.provision" so it can sync folders properly.



# Run GitHub CI in VirtualBox via Vagrant

Use this action to run your CI in `FreeBSD`, `OpenBSD`, `NetBSD`, etc.

# Sample workflow

```yml
name: Build
on: [push]
jobs:
  build-on-openbsd:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v2

      - uses: kentahikaru/github-action-vagrant--vagrantfile-from-repository@v1
        with:
          run: |
            export AUTOCONF_VERSION=2.69
            export AUTOMAKE_VERSION=1.16
              
            export CFLAGS='-I/usr/local/include -L/usr/local/lib'
              
            if [ ! -f /usr/local/lib/libiconv.so ] ; then
                sudo ln -s /usr/local/lib/libiconv.so.* /usr/local/lib/libiconv.so
            fi
              
            run sudo pkg_add automake-1.16.2
            
            run ./autogen.sh
            run ./configure --prefix=/usr
            run make
            run sudo make install
            run file /usr/bin/ctags
            run ctags --version
```

|option|required|overview|
|-|-|-|
|`runs-on`|✔︎|must be `macos-*`|
|`run`|✔︎|the commands you want to run in vm|



All the `GITHUB_*` environment variables are passed into the VM.

The host machine folder `${GITHUB_WORKSPACE}` has been synchronized into the VM folder `~/${GITHUB_REPOSITORY}`.

The working dir of VM is `~/${GITHUB_REPOSITORY}`

you can use `run` command in `run` option, the defination of `run` command as follow:
```
run() {
    printf "\033[0;35m==>\033[0m \033[0;32m%b\n\033[0m" "$*"
    $@
}
```

# Under the hood

`GitHub Actions` only supports `Ubuntu`, `Windows` and `macOS` out of the box.

However, the `macOS` support virtualization. It has `VirtualBox` and `Vagrant` installed.

So, we can run the `FreeBSD`, `NetBSD`, `OpenBSD` OS in `VirbualBox` on `macOS`.











