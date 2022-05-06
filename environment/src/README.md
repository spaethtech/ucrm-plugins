#### environment/src

> _This folder is excluded from source control by default and should remain that way._

Maps to `/home/vagrant/src/` inside the Vagrant machine, which is mounted to `/data/ucrm/data/plugins` inside the UCRM
docker container.

This folder contains the deployed contents of the Plugin and can be useful for things such as:
- Verifying successful execution of the Plugin hooks.
- Examining Plugin generated content.
- and much more...

This folder will be empty after installation of the Vagrant system, but will update (via the SMB share) as Plugins added
or removed in the UCRM system.
