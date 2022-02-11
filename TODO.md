# Complete scripts documentation @full-docs
Besides mkbsdrd, all scripts are too succinctly documented; we want a
full documentation; this should also be automatically included in
the README.md via the Makefile, e.g. ``make update-doc``.

# mkbsdrd to use UFS writing on Linux @ufs-write
Through ``./rdsetroot.c`` we can split open a ``bsd.rd`` from
Linux; the filesystem it contains is an UFS one, for which we
only have an unstable write on Linux, which is thus often disabled
by default.

Given how little we tweak the file, it may be a better option
than to rely on a remotely available OpenBSD...

# ovh-do: update key @ovhdo-ssh-key-update
If a key "ovh-do-key" is already registered, and if the default
key found in ``$HOME/.ssh/`` doesn't match its value, we'd want
to update the OVH key.

# More flexibility toward OVH's "stock" Debian @more-flexible-debian-dep
E.g.:

  - Default Debian user name hardcoded (``debian``);
  - Assuming a fixed number of GRUB entries to automatically load
  the OpenBSD one;
  - GRUB timeout arbitrarily set to 10s.

# install.conf generation @gen-install-conf
This may be performed with an external script; we could find a way
to keep the same mechanism than ``ovh-do`` to retrieve the key.

# Uniform/better logging @logging

# Parallelizing setup-debian & mkbsdrd @parallel-setup
Both operations are performed sequentially but could be performed in
parallel. This should slightly speed up the build.

# Avoid rebuilding the bsd.rd @pre-made-bsd.rd
Perhaps an option to use skip building a bsd.rd would be useful,
especially for tests purposes; we could use a checksum of the
install.conf to see whether it should be rebuilt.

# ovh-do waitfortask timeout @waitfortask-timeout
