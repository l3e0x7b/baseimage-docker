# A minimal Debian base image modified for Docker-friendliness

Optimized for users in mainland China

<a name="building"></a>
## Building the image yourself

Clone this repository:

    git clone https://github.com/l3e0x7b/baseimage-docker.git
    cd baseimage-docker

Start a virtual machine with Docker in it. You can use the Vagrantfile that we've already provided.

First, install `vagrant-disksize` plug-in:

    vagrant plugin install vagrant-disksize:

Then, start the virtual machine

    vagrant up
    vagrant ssh
    cd /vagrant

Build the image:

    make build

If you want to call the resulting image something else, pass the NAME variable, like this:

    make build NAME=joe/baseimage

To verify that the various services are started, when the image is run as a container, add `test` to the end of your make invocations, e.g.:

    make build NAME=joe/buster test


<a name="removing_optional_services"></a>
### Removing optional services

The default baseimage-docker installs `syslog-ng`, `cron` and `sshd` services during the build process.

In case you don't need one or more of these services in your image, you can disable its installation through the `image/buildconfig` that is sourced within `image/system_services.sh`.  Do this at build time by passing a variable in with `--build-arg`  as in `docker build --build-arg DISABLE_SYSLOG=1 image/`, or you may set the variable in `image/Dockerfile` with an ENV setting above the RUN directive.

These represent build-time configuration, so setting them in the shell env at build-time [will not have any effect](https://github.com/phusion/baseimage-docker/issues/459#issuecomment-439177442).  Setting them in child images' Dockerfiles will also not have any effect.)

You can also set them directly as shown in the following example, to prevent `sshd` from being installed into your image, set `1` to the `DISABLE_SSH` variable in the `./image/buildconfig` file.

    ### In ./image/buildconfig
    # ...
    # Default services
    # Set 1 to the service you want to disable
    export DISABLE_SYSLOG=0
    export DISABLE_SSH=1
    export DISABLE_CRON=0

Then you can proceed with `make build` command.

<a name="conclusion"></a>
## Conclusion

 * Using baseimage-docker? [Tweet about us](https://twitter.com/share) or [follow us on Twitter](https://twitter.com/phusion_nl).
 * Having problems? Want to participate in development? Please post a message at [the discussion forum](https://groups.google.com/d/forum/passenger-docker).
 * Looking for a more complete base image, one that is ideal for Ruby, Python, Node.js and Meteor web apps? Take a look at [passenger-docker](https://github.com/phusion/passenger-docker).
 * Need a helping hand? Phusion also offers [consulting](https://www.phusion.nl/consultancy) on a wide range of topics, including Web Development, UI/UX Research & Design, Technology Migration and Auditing. 

[<img src="https://avatars.githubusercontent.com/u/830588?s=200&v=4">](https://www.phusion.nl/)

Please enjoy baseimage-docker, a product by [Phusion](http://www.phusion.nl/). :-)
