Data Repository Example for OpenStack External Testing
======================================================

This is an example Data Repository to use with OpenStack External Testing Platform Installer.

DO NOT fork this repository.

It is intended to be copied to some private location (possibly a
private GitHub repository, possibly somewhere else private in your organization). This
repository will contain private SSH keys and other sensitive information.

9/4/2015 It has been updated to follow the [project-config file layout](https://git.openstack.org/cgit/openstack-infra/project-config/tree/)

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
   job artifacts to. This is also known as the Log server. You can set one up using [this script]
   (https://github.com/rasselin/os-ext-testing/blob/master/puppet/install_log_server.sh).

9. Change the mysql passwords in `$MYSQL_ROOT_PASSWORD` with the (current) root mysql password and
   `$MYSQL_PASSWORD` with the desired nodepool mysql password.

10. Optionally change the `$GIT_EMAIL` and `$GIT_NAME` to a different name. These are used by zuul to manage
    internal git trees.

11. Optionall add your SMTP server in `$SMTP_HOST` if you would like to use zuul's e-mail notification features.

12. Copy the `nodepool/nodepool.yaml.sample` to  `nodepool/nodepool.yaml` and modify as needed. Some common properties
   are delimited by <%=  %> are also set in the vars.sh file and need to be manually. These include username and password.
   You can find the full configuration details in the [Nodepool manual](http://docs.openstack.org/infra/nodepool/).
   In particular, update the mysql_password to `$MYSQL_PASSWORD`. Choose a new time for the image-update cron job.
   Setup the OpenStack provider username, password, and auth-url. Finally, synchronize the jenkins environment to what
   you configured in vars.sh.

13. Copy the `zuul/layout.yaml.sample` file to `zuul/layout.yaml` and update it to your needs. You can find the full
    configuration details in the [Zuul manual](http://docs.openstack.org/infra/zuul/). In particular, update the portions
    that have 'myvendor' and configure the e-mail addresses for merge-failures.

14. Adjust the jenkins jobs in `jenkins/jobs/` to your needs. You can find the full configuration details in the
    [Jenkins Job Builder manual](http://docs.openstack.org/infra/jenkins-job-builder/)

15. Setup an intial set of nodepool scripts and elements. Start by cloning OpenStack's
    [project-config](https://git.openstack.org/cgit/openstack-infra/project-config/) and copy the contents of
    that repo's `nodepool/elements` to your repo's `nodepool/elements`. Optionally do the same for the `nodepool/scripts`
    folder. You may have to change these elements to work in your environment. If so, see this
    [README](http://git.openstack.org/cgit/openstack-infra/project-config/tree/nodepool/elements/README.rst) for help.


Migrate to project-config
-------------------------

If you have not yet migrated to the 'project-config' layout, you are strongly encouraged to do so.
Fortunately, it is a simple task:

1. Copy the files in `etc/jenkins_jobs/config` to jenkins/jobs

2. Copy `etc/zuul/layout.yaml` to `zuul/layout.yaml`

3. Copy the files and directories in `etc/nodepool` `nodepool/`
