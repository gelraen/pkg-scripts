# Helper scripts for local poudriere

This repository contains a few small scripts that aid in running a poudriere
instance on a local machine.

It it assumed that you already have poudriere configured to build packages for
the appropriate FreeBSD version and pkg configured to use that packages.

`update_repo` updates the ports tree, grabs the list of locally installed
packages (filtering out those installed automatically as a dependency) and
kicks off a rebuild of that packages (poudriere skips rebuilding those that
are already up to date). It can be safely invoked from your `/etc/crontab` as
installed packages are not affected in any way.

`add_package` can be used to build a package for something that is not
currently present in your local repository. Just give it a bunch of origins
("category/portname") as command-line args.

## Tips & tricks

* You can easily make poudriere build all packages with your locally configured
options: `sudo ln -s /var/db/ports /usr/local/etc/poudriere.d/options`
