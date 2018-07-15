[This](https://github.com/agenosov/ansible_notes/tree/master/warm_up) is a simple demo of using Ansible. No 3-rd party packages will be installed. It's just to illustrate how to:

- use a custom configuration file
- work with inventory and organize hosts in groups
- gather facts about remote nodes, including *local* facts provided by some managed system (in this *toy* example this is just a script for providing some data)
- use templates of config files (instead of installing some real packages, just install configs with data populated from inventory variables)
- perform actions on target hosts, local actions and also use the *delegate to*.

### Organizing inventory

Our demo inventory will hold both hosts which aren't in any particular group and hosts which are grouped by criteria.

According to [example from best practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#group-and-host-variables) hosts can be organized in groups either on geographic criteria or by some unique configuration options. We are going to organize hosts by roles (later we'll do the same by using Ansible roles feature). 

We'll have the following groups of hosts:
* control nodes
* analytics nodes
* agents nodes.

One host is going to be in all groups mentioned above. And we'll see that *for this host variables will come from all groups*.

### Facts gathering

Setting up an option (*gathering = smart*) to not recollecting facts from already discovered hosts allows to minimize the amount of information transferred during subsequencial connections to the same hosts during a play. 
This improvement could be really tangible (comment this option in the configuration file and you'll see that Ansible gathers facts from the same hosts when processing each *hosts* section).

In this demo I use discovered facts to determine which package manager is used on a target nodes. It could be necessary in order to know which types of packages, for instance *rpm* or *deb*, to install.

Also during playbook running we upload a special file to all target nodes in order to collect *facts specific to our environment* (i.e. this script is capable of gathering such specific data). Ansible documentation calls such data *local facts* which means these data are not related to a whole system but specific for concrete environment or product.
