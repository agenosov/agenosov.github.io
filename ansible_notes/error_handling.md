For multi-node deployments it makes sense to consider using the ['any_errors_fatal' option](https://docs.ansible.com/ansible/latest/user_guide/playbooks_error_handling.html#aborting-the-play) in conjunction with some critical tasks.

For instance, you are going to install packages as one of the steps (let's call it *step A*) and afterthat to perform some communication between installed instances (call it *step B*). 

In such scenario, if during the installation step there was an error on some host (due to a conflict of resolving dependencies between packages, as an example), we would come up with an error only during the *step B*.

It's much better to fail overall deployment early.
