Kiji Top Level
==============

[![Build Status
](https://travis-ci.org/kijiproject/kiji-top-level.svg?branch=master)
](https://travis-ci.org/kijiproject/kiji-top-level)

Top level repository that contains all of Kiji sub projects.

Testing
-------

This repository contains configuration for running sub project integration tests across
a number of different, supported platforms.  This is accomplished with the Travis build
service (click the badge above) and their [build matrix support
](http://docs.travis-ci.com/user/build-configuration/#The-Build-Matrix).

This build is broken into two steps, see `provisioning/install` and `run-tests`. The
list of tested platforms is in `.travis.yml`.

To investigate build failures, one can locally recreate the test environment with
[Vagrant](http://www.vagrantup.com/), using the virtualbox, vmware fusion or vmware
workstation providers.  Beyond vagrant's usual configuration options, one may select a
default provider by echoing its name to `.vagrant/provider`. The default is virtual box.

To always use vmware fusion (for example):
```
$ echo vmware_fusion > .vagrant/provider
```

After installation and configuration, one can create the environment and run the tests as
follows.  Notice the choice of platform with the `PLATFORM` environment variable.

```bash
$ PLATFORM=cdh4.4.0 vagrant up
$ vagrant ssh
vagrant@kiji:~$ cd kiji
vagrant@kiji:~/kiji$ ./run-tests
vagrant@kiji:~/kiji$ exit
$ vagrant halt  # Or `vagrant destroy -f`
```
