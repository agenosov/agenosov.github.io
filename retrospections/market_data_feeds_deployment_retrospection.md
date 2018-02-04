When I worked in investment company I was part of development team for FIX connectivity services.
The C++ part of these services were a market data feeds as a Windows services which were deployed to two production servers.

**Deployment process was extremely unconvenient as it was manual**. In order to release new functionality, support team had to manually uninstall old services,
then install a new one, after that ensure that everything works fine and in case of serious problems make a rollback to previous release, i.e. perform all manual steps in a reverse order.

Now I see that there were *two improvement points*:
1. provide MSI installers instead of self-executable archives made with NSIS
2. make deployment of new functionality automatically.

I don't know how I would make the second point in 2012 when Ansible was just in the beginning, but **now I definetly would deploy services with Ansible thanks to it's ability to manage Windows hosts**.

**TODO**: link to a demo of working with MSI packages via Ansible.