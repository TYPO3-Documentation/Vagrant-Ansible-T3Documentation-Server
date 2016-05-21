
.. include:: ../Includes.txt

======================================
The Playbook
======================================



Codename:
   Vagrant-Ansible-T3Documentation-Server


About Creating Playbooks
========================

- https://www.theodo.fr/blog/2015/10/best-practices-to-build-great-ansible-playbooks/

- http://blog.cliffano.com/2014/04/06/human-readable-ansible-playbook-log-output-using-callback-plugin/




Vagrantfile
===========

-  uses a plain Debian Jessie ("debian/jessie64")

-  jenkins: guest: 8580, `host: 8580 <http://127.0.0.1:8580>`__

-  web server: guest: 80, `host: 8510 <http://127.0.0.1:8510>`__

-  private_network 192.168.85.10, needed for Virtualbox NFS share

-  config.vm.synced_folder :file:`.hibernate/var/cache/apt/archives`,
   :file:`/var/cache/apt/archives`, type: "nfs"


Hostnames
=========

We assume **docs.typo3.org.local** and **docs.typo3.org.local**.
See :file:`ssh-config`)::

   Host docs-typo3-org-local
     HostName docs.typo3.org.local
     User vagrant
     Port 22
     IdentityFile /abs/path/to/.vagrant/machines/default/virtualbox/private_key

   Host docs-typo3-org-remote
     HostName docs.typo3.org.remote
     User vagrant
     Port 22
     IdentityFile /abs/path/to/.vagrant/machines/default/virtualbox/private_key


Running The Playbook
=====================

There is a configuration :file:`ansible-development-hosts.ini`
for provisioning development hosts. They are located in the
group "dev". One of those hosts is 'docs.typo3.org.local'.

And there is a configuration :file:`ansible-production-hosts.ini`
for provisioning production hosts. They are located in the group
'production'. One of those hosts ist 'docs.typo3.org.remote'.

When running the playbook the appropriate configuartion has to be selected.

To provision development hosts run::

   ansible-playbook -i ansible-development-hosts.ini playbooks/main.yml

:file:`ansible-development-hosts.ini` is the default hostfile
so for the development hosts you may just run::

   ansible-playbook playbooks/main.yml

To provision production hosts::

   ansible-playbook -i ansible-production-hosts.ini  playbooks/main.yml


Specialized Runs Of The Playbook
================================

The basic settings for the playbook are made in :file:`group_vars/all.yml`.
Additionally there are settings for 'dev' hosts in :file:`group_vars/dev.yml`
and productions hosts in :file:`group-vars/production.yml`.


Ansible Plugins
===============

human_log
---------

Describe the ansible callback plugin :file:`human_log.py` here.

