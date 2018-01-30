Make a simple demo, without installing any 3-rd party packages, just to illustrate:

- using configuration
- working with inventory (several hosts with different variables defined per host)
- gathering facts about remote nodes, including *local* facts provided by some managed system (make a toy example - just a script for providing some data)
- using of template config files (instead of installing some real packages, just install configs with data populated from inventory variables)

Perform actions on target hosts, local actions and also use the *delegate to*.

Also make an example cleanup of what was previously installed, i.e. example of performing full revert.