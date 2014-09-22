# CoreOS Vagrant

This repo provides a template Vagrantfile to create a CoreOS virtual machine
using the VirtualBox software hypervisor.  After setup is complete you will have
a single CoreOS virtual machine running on your local machine.

## Streamlined setup

```
brew install caskroom/cask/brew-cask
brew cask install vagrant virtualbox
git clone https://github.com/haf/vnext-environment.git
cd vnext-environment
cp config.rb.sample config.rb # uncomment the auto-token code at the start of file
cp user-data.sample user-data
vagrant up
vagrant ssh core-01 -c 'docker --version'
alias docker='docker -H tcp://127.0.0.1:2375
brew versions docker
(cd /usr/local/Library
 git checkout c6e04e3 # or whatever the above command displayed as the docker ver
)
docker info # this displays the output of the core-01 docker config,
            # through the alias set up above (you can add it to your
            # shell profile now
docker pull centos # get latest baselines
```

Now, get started [using CoreOS][using-coreos]

[virtualbox]: https://www.virtualbox.org/
[vagrant]: https://www.vagrantup.com/downloads.html
[using-coreos]: http://coreos.com/docs/using-coreos/

#### Shared Folder Setup

Automatic.

#### Provisioning with user-data

The Vagrantfile will provision your CoreOS VM(s) with [coreos-cloudinit][coreos-cloudinit] if a `user-data` file is found in the project directory.
coreos-cloudinit simplifies the provisioning process through the use of a script or cloud-config document.

To get started, copy `user-data.sample` to `user-data` and make any necessary modifications.
Check out the [coreos-cloudinit documentation][coreos-cloudinit] to learn about the available features.

[coreos-cloudinit]: https://github.com/coreos/coreos-cloudinit

#### Configuration

The Vagrantfile will parse a `config.rb` file containing a set of options used to configure your CoreOS cluster.
See `config.rb.sample` for more information.

## Cluster Setup

Launching a CoreOS cluster on Vagrant is as simple as configuring `$num_instances` in a `config.rb` file to 3 (or more!) and running `vagrant up`.
Make sure you provide a fresh discovery URL in your `user-data` if you wish to bootstrap etcd in your cluster.

## New Box Versions

CoreOS is a rolling release distribution and versions that are out of date will automatically update.
If you want to start from the most up to date version you will need to make sure that you have the latest box file of CoreOS.
Simply remove the old box file and vagrant will download the latest one the next time you `vagrant up`.

```
vagrant box remove coreos --provider virtualbox
```

## Docker Forwarding

By setting the `$expose_docker_tcp` configuration value you can forward a local TCP port to docker on
each CoreOS machine that you launch. The first machine will be available on the port that you specify
and each additional machine will increment the port by 1.

Follow the [Enable Remote API instructions][coreos-enabling-port-forwarding] to get the CoreOS VM setup to work with port forwarding.

[coreos-enabling-port-forwarding]: https://coreos.com/docs/launching-containers/building/customizing-docker/#enable-the-remote-api-on-a-new-socket

Then you can then use the `docker` command from your local shell by setting `DOCKER_HOST`:

    export DOCKER_HOST=tcp://localhost:2375
