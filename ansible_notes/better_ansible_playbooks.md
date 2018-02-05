Despite of the fact that Ansible is positioned as a simple IT automation platform, managing the real project with Ansible playbooks turns out to be a task full of nuances and unpredictable issues.

As with any programming language and technology, it's necessary to have some conventions, best practices and to know about pitfalls.

When a system which is managed by Ansible playbooks gets complex while evolving, it leads to complicating inside playbooks itself.
When initially simple playbook starts to contain more and more tasks, 
it's the signal to split it into several blocks of tasks to be included statically or dynamically into the main playbook.

Even a separate list of tasks may become too hard to maintain, for example if several tasks in a play are to be executed conditionally.
In this situation it's preferrable to wrap these tasks within a block and **apply condition to whole block**.
It would be much clearer than to have a list of tasks and try to support same conditions for each task in a list.
Also using a block is convenient when you need to [guarantee that in case of error during playbook running a cleanup would be performed](http://github.com/agenosov/ansible_notes/tree/master/error_handling).

Sometime it's required to apply different *processing strategies* for different lists of tasks,
and in this case instead of having many *hosts* sections inside the main playbook it's far better to **import separate playbooks**.

As with controlling warnings from your compiler, it's necessary to [pay attention to warnings which Ansible may produce](http://github.com/agenosov/ansible_notes/tree/master/fixing_warnings).

To wrap up:

* keep the main playbook simple in order to not loose the big picture of what's going on.

* use a modular approach to organize all your playbooks.

* use the *block* construction to simplify working with conditions and to guarantee error handling.

* keep tasks dedicated to separate groups of hosts in different playbooks.

Paying attention to the quality of code in Ansible playbooks is critical for providing it's maintainability.