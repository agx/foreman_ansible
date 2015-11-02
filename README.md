# Foreman Ansible :arrow_forward:

[Foreman](http://theforeman.org) integration with Ansible. For now, it's just importing facts and not meant for general use.

** Warning - this project hasn't been released yet, and might change significantly. Please don't use in production. **

## Basic usage

Deploy `extras/foreman_callback.py` as a callback on your Ansible installation. For instance, put in your `ansible.cfg`:

```
callback_plugins = ~/.ansible/plugins/callback_plugins/
bin_ansible_callbacks = True
```

And copy `extras/foreman_callback.py` from this repo to `~/.ansible/plugins/callback_plugins/`. That's it!

Now, every time you run `ansible -m setup $HOSTNAME`, Ansible will automatically submit facts for $HOSTNAME to Foreman.

In Foreman, you should add whatever Ansible hosts you want to submit facts from to the Setting (Administer > Settings, Puppet tab - I know..)) 'trusted_puppetmaster_hosts'.

#### Demo

![demo gif](http://i.imgur.com/mlnVFJj.gif)

### Extra

If the Foreman setting 'create_new_host_when_facts_are_uploaded' is true, and $HOSTNAME doesn't exist in Foreman, it will autocreate that host in Foreman. If it already exists, it will update the facts.

Similarly, the Foreman setting 'ignore_puppet_facts_for_provisioning' is set to false, facts related to interfaces will update the interfaces of $HOSTNAME in Foreman.

### Devs

Send a POST request to /api/v2/hosts/facts with the format you can see [in the API docs](http://theforeman.org/api/1.9/apidoc/v2/hosts/facts.html).

Facts must contain the output of `ansible -m setup $HOSTNAME`, plus a '_type' and '_timestamp' top level keys. You can see an example on test/fixtures/sample_facts.json in this repository.

After that request, you should have a host registered in Foreman with the Ansible facts. It takes into account some facter and ohai facts if these are available on the system as well.

## Contributing

Fork and send a Pull Request. Thanks!

## Copyright

Copyright (c) Daniel Lobato Garcia

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.