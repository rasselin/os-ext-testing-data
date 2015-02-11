Data Repository Example for OpenStack External Testing
======================================================

This is an example Data Repository to use with OpenStack External Testing Platform Installer.

DO NOT fork this repository.

It is intended to be copied to some private location (possibly a
private GitHub repository, possibly somewhere else private in your organization). This
repository will contain private SSH keys and other sensitive information.

Manual Instructions
-------------------

Follow these manual instructions to get your data repository set up:

1. Copy the repository somewhere private.

2. Copy the **private** SSH key that you submitted when you registered with the upstream
   OpenStack Infrastructure team into somewhere in this repo.

3. Create an SSH key pair that you will use for Jenkins. This SSH key pair will live
   in the `/var/lib/jenkins/.ssh/` directory on the master Jenkins host, and it will
   be added to the `/home/jenkins/.ssh/authorized_keys` file of all slave hosts::

    ssh-keygen -t rsa -b 1024 -N '' -f jenkins_key

   Once you do the above, copy the `jenkins_key` and `jenkins_key.pub` files into your
   data repository.

4. Copy `vars.sh.sample` to `vars.sh` and open up `vars.sh` in an editor.

5. Change the value of the `$UPSTREAM_GERRIT_USER` shell
   variable to the Gerrit username you registered with the upstream OpenStack Infrastructure
   team [as detailed in these instructions](http://ci.openstack.org/third_party.html#requesting-a-service-account)

6. Change the value of the `$UPSTREAM_GERRIT_SSH_KEY_PATH` shell variable to the **relative** path
   of the private SSH key file you copied into the repository in step #2.

   For example, let's say you put your private SSH key file named `mygerritkey` into a directory called `ssh`
   within the repository, you would set the `$UPSTREAM_GERRIT_SSH_KEY_PATH` value to
   `ssh/mygerritkey`

7. If for some reason, in step #3 above, you either used a different output filename than `jenkins_key` or put the
   key pair into some subdirectory of your data repository, then change the value of the `$JENKINS_SSH_KEY_PATH`
   variable in `vars.sh` to an appropriate value.

8. Change the value of the `$PUBLISH_HOST` to the host (without https:// prefix) you will publish
   job artifacts to.

9. Examine the files in `etc/jenkins_jobs/config` and modify as you need. Refer to this blog post
   for more information.

10. Example the `etc/zuul/layout.yaml` file and ensure you set up each upstream project that your
   testing system intends to run Jenkins jobs for.

11. Copy the `etc/nodepool/nodepool.yaml.erb.sample` to  `etc/nodepool/nodepool.yaml.erb` and modify as needed. Some common properties
    are set in the vars.sh file and populated by puppet.
