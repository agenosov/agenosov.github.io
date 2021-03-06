One of the Ansible modules features is idempotence, i.e. a module do nothing with an object it manages if the current state already matches the desired state.

This is cool not only because of not performing excess activity.

Thanks to that fact I discovered a bug in one of our init.d scripts.

I was testing the downgrade scenario for our product.

On Debian and it's derivatives, during a downgrade there is a necessity to
remove a current version of package before installing an oldest version from
local .deb package (**TODO**: link to a note with explanation why...).

We have an internally built and packaged Postgresql, and Postgres server is started only by a script which creates DB during initial deployment. 
Because of this, we didn't started the server explicitly from Ansible playbooks.
...Until we encountered with some another issue.

On CentOS and other rpm-based analogs there is a *systemd*.
It manages inter-services dependencies, and thanks to that fact, as our another service is dependent from the Postgresql, systemd launched Postgresql when we started our service from Ansible playbook.

But we also supported a Debian derivative *without systemd*. For this kind of system I decided to just check during playbook run if systemd is available and if not, to explicitly start service for Postgresql. And here there was another surprise...
I just added the task for it:

    - service: name=custom-postgresql state=started enabled=yes

and expected that everything will be fine.

But after successful downgrading and after deployment was completed I **didn't see that Postgresql is running!**
I had to launch it manually!

Thanks to ability to debug any Ansible module remotely, I figured out what was wrong.
During debugging the Python script of managing Linux services from Ansible, it turned out that **before actually launching a service, there is a check of current status of service**. If the command of checking service's status returns zero, it means that service is already running and there is nothing to do.

I checked the code of init.d script for Postgresql from our package and it turned out that for checking status there was no other return codes except zero, i.e. if service wasn't launched script notified about this into console but an error code was zero.

That's why I saw that after performing the task of launching Postgres the *changed* field in the output from module's execution was *false*.

After fixing the error in init.d script everything became fine - Postgres was active after downgrading. Thanks for Ansible's idempotence:)
