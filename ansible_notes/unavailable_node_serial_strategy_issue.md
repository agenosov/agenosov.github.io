I had a real use case when _main playbook_ included another playbook which defined ['serial: 1' in order to process hosts one by one](https://docs.ansible.com/ansible/latest/user_guide/playbooks_delegation.html#rolling-update-batch-size) before going further (i.e. continue the _main playbook_). And it was **absolutely vital to complete main play** despite of the fact that some hosts were marked as failed while executing the _included playbook_.

When using serial strategy, **if entire batch of hosts failed** (due to they are unreachable or due to some processing error) then **Ansible interrupts all remaining plays**.

While it makes sense for some cases, there're also situations when such behaviour is absolutely unexpectable.
Ansible offers a way to [control a certain threshold of failures](https://docs.ansible.com/ansible/latest/user_guide/playbooks_delegation.html#maximum-failure-percentage) in order to abort the play early... but there's no way to control whether it's acceptable to continue plays if current batch of hosts failed.

I had to introduce a [special option](https://github.com/ansible/ansible/pull/40271) to fix it. [Here](https://github.com/agenosov/ansible_notes/tree/master/unavailable_node_issue) is an example of how it works.
