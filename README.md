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

## Setting up poudriere

* Install it from ports or official packages:
`make -C /usr/ports/ports-mgmt/poudriere` or `pkg install poudriere`
* Edit `/usr/local/etc/poudriere.conf`. Most important pieces are paths to
directories where you want it to store the data.
* Create a new ports tree for it: `poudriere ports -c -p default -m portsnap`.
Ports tree "default" will use `portsnap`, so if you want to have local patches
replace it with `svn` or `git`.
* Create a new build jail. If you run a FreeBSD release:
`poudriere jail -c -j your_jail_name -v $(uname -r)`. If you run FreeBSD from
a stable branch, use the same command as above, but specify a version for `-v`
flag manually, e.g. `10.1-RELEASE` if you run `10.1-STABLE`.

## Tips & tricks

* You can easily make poudriere build all packages with your locally configured
options: `sudo ln -s /var/db/ports /usr/local/etc/poudriere.d/options`
