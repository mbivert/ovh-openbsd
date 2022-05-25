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

# @getbsd-signify
We only check the SHA; we could also check the signature,
eventually making it optional.

# @full-auto-mkvm
See if we can find a way to fully automatize the VM creation,

# @multiple-vms-mkvm
Shouldn't be too hard: just run the code on multiple VM configuration
(ie. set of shell variables). A difficulty would be to interconnect
them properly. Setting up a local mirror copy would now make more sense.

# @patchbsdrd-input
There's some weird behavior on patchbsdrd depending on how input
bsd.rd is provided

The following fails on the remote gzip -d

  cat $fn | patchbsdrd

But the following works:

  patchbsdrd < $fn

And within patchbsdrd, using cat - or cat /dev/stdin seems to
also alters the remove ``gzip -d`` success when input is provided
on stdin.

# ovh-do: update key @ovhdo-ssh-key-update
If a key "ovh-do-key" is already registered, and if the default
key found in ``$HOME/.ssh/`` doesn't match its value, we'd want
to update the OVH key.

# ovh-do: credentials managements
There are some routes (new? Api status is "ALPHA") allowing to list
applications, credentials for applications, etc.

Maybe we could find a way to leverage/simplify credential managements
for use with ovh-do. Likely, for (re)bootstraping, we'll need to launch
a browser. See:

	GET /me/api/application
		Returns a list of applicationId (integers)
	DELETE /me/api/application/{applicationId}
		Remove given application
	GET /me/api/application/{applicationId}
		Retrieves names, description, etc.
	GET /me/api/credential
		Has an applicationId parameters and a way to pre-filter results;
		return a list of credentialId (integers)
	DELETE /me/api/credential/{credentialId}
	GET /me/api/credential/{credentialId}
		To respectively delete/retrieve the corresponding credentials
		details (last usage, etc.)
	PUT /me/api/credential/{credentialId}
		From the documentation, doesn't seem to allow updating usage
		period.

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
