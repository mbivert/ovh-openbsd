# Introduction
This repository contains a few tools to help automatically
install an (amd64) OpenBSD on an OVH VPS. The process is
cumbersome, but works reasonably well.

For more details, see this [blog post][mb-openbsd-ovh].

# Requirements
Before starting, you'll need:

  - an amd64 OVH VPS that can be fully erased;
  - a Linux system from which to run all this;
  - an existing remotely (SSH) available OpenBSD, used to tweak
  the default ``bsd.rd`` so as to include an ``install.conf``;
  see [autoinstall(8)][oman-8-autoinstall];
  - OVH API credentials to use the API: we rely on the
  [default mechanism][gh-py-ovh-conf] provided by the official
  OVH API Python wrapper.

The tools being rather sharp, it's advised to take the time
to read the code and/or the [blog post][mb-openbsd-ovh].

# Files
## ./rdsetroot.c
A quickly patched [rdsetroot(8)][oman-8-rdsetroot] to compile on Linux. So
far unused. See ``TODO.md:/^#.*@ufs-write``.

## ./bin/ovh-do
Python wrapper to provide a partial access to the OVH API to other
(shell) scripts.

    $ ovh-do -h
    ovh-do <vps-id> <cmd=ls-imgs|get-console|ls-keys|ls-key|set-key|ls-ips|setup-debian> [key]

## ./bin/mkbsdrd
Creates a ``bsd.rd`` embedding a specific ``install.conf`` file, using
the remote OpenBSD to edit the default ``bsd.rd``. See also [upobsd][github-upobsd].

    $ mkbsdrd -h
    NAME
    	mkbsdrd

    SYNOPSYS
    	mkbsdrd [-h]
    	mkbsdrd [-v version ] [-a arch] [-m mirror] [-p mpath] [-i install.conf]
    	   [-r remote] [-c conf] [-w rwd] [path/to/bsd.rd]

    DESCRIPTION
    	mkbsdrd fetch and augment an OpenBSD bsd.rd with the given install.conf,
    	where:

    	  version       $version   OpenBSD version      (default: 7.0)
    	  arch          $arch      Architecture to use  (default: amd64)
    	  mirror        $mirror    OpenBSD mirror       (default: https://cdn.openbsd.org)
    	  mpath         $mirror    Mirror path          (default: pub/OpenBSD)
    	  install.conf  $install   Path to install.conf (default: ./install.conf)
    	  remote        $remote    Remote OpenBSD (ssh) (default: )
    	  rwd           $rwd       Remote work dir      (default: /root/mkbsdrd/)

    	-c allows to specifies a configuration file (shell script) that
    	is sourced, allowing to set/override all variables mentioned above.

    	If no bsd.rd output path is specified, resulting bsd.rd is printed
    	to stdout.

    SEE ALSO
    	autoinstall(8)

## ./bin/install-openbsd-to
Shell script installing an OpenBSD to the given VPS IP. It assumes
that the VPS has been:

  - configured to be an OVH "stock" Debian image;
  - with a password-less SSH access to the default ``debian`` user;
  - and a GRUB2 setup.

By default, the OpenBSD is set with a root of ``hd0,gpt1``; the bsd.rd
is likely to have been created by ``mkbsdrd`` and the debian setup
via ``ovh-do <vps-id> setup-debian``:

    $ install-openbsd-to -h
    install-openbsd-to <vps-id> <bsd.rd> [grub-root-id]

## ./bin/ovh-install-openbsd
The main wrapper, that uses the three previous scripts to perform
the complete automatic installation.

    $ ovh-install-openbsd -h
    ovh-install-openbsd [-g grub-root-id] [-i vps-ip] <vps-id> [mkbsdrd-opts]

[mb-openbsd-ovh]:     https://tales.mbivert.com/on-openbsd-ovh-vps-automatic-installation/
[oman-8-autoinstall]: https://man.openbsd.org/autoinstall.8
[oman-8-rdsetroot]:   https://man.openbsd.org/rdsetroot.8
[gh-py-ovh-conf]:     https://github.com/ovh/python-ovh#configuration

[github-upobsd]: https://github.com/semarie/upobsd/
