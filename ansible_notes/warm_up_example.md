This is a simple demo of using Ansible. No 3-rd party packages will be installed. It's just to illustrate how to:

- use a custom configuration file
- work with inventory and organize hosts in groups
- gather facts about remote nodes, including *local* facts provided by some managed system (make a toy example - just a script for providing some data)
- use templates of config files (instead of installing some real packages, just install configs with data populated from inventory variables)
- perform actions on target hosts, local actions and also use the *delegate to*.

### Organizing inventory

Our demo inventory will hold one host which isn't in any particular group.
All other hosts will be grouped by criteria. According to [example from best practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#group-and-host-variables) hosts can be organized in groups either on geographic criteria or by some unique configuration options. We are going to organize hosts by roles (later we'll do the same by using Ansible roles feature). We'll have the following groups of hosts:
* control nodes
* analytics nodes
* agents nodes.

One host is going to be in all groups mentioned above. And we'll see that *for this host variables will come from all groups*. 
