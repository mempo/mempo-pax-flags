#!/bin/bash -e
# see: https://wiki.debian.org/grsecurity
# please see also:
# wiki.debian.org/Mempo
# wiki.debian.org/SameKernel

# needed package: attr - install this
#
# this programs do NOT have proper protection form kernel, it is disabled to let them run!
# mempo.org project will aim to remedy this one day by turning runtime-JIT to separated precompile

echo "Installing the PAX flags for grsecurity. In case of any problems, contact us: mempo.org ; wiki.debian.org/Mempo"

function failure() {
	echo "Script ended with error. Contact us for help: mempo.org. We are on IRC chat as written on that page, but wait 24 hours for the reply there."
	exit 1
}

function set_pax_flag_on() {
	the_flag="$1"
	the_files="$2"

	found=1
	stat "$the_files" &> /dev/null || found=0

	if [[ "$found" = "1" ]]
	then
		echo "... Setting $the_flag on $the_files"
		setfattr -n user.pax.flags -v "$the_flag" "$the_files" || { echo "Failed to set $the_flag on $the_files" ; failure ; }
	fi
}

set_pax_flag_on "mr" /usr/lib/xulrunner-*/xulrunner-stub 
set_pax_flag_on "mr" /usr/lib/iceweasel/iceweasel # debian 7
set_pax_flag_on "mr" /usr/lib/icedove/icedove-bin
set_pax_flag_on "mr" /usr/lib/iceowl/iceowl-bin # debian 6

# tricky part is to run this in between upgrade of kernel for reinstalling/updating grub
set_pax_flag_on "m"  /usr/sbin/grub-*
set_pax_flag_on "m"  /usr/sbin/grub-mkdevicemap
set_pax_flag_on "m"  /usr/bin/grub-mount
set_pax_flag_on "m"  /usr/bin/grub-script-check
set_pax_flag_on "m"  /usr/lib/grub/i386-pc/grub-setup

set_pax_flag_on "m"  /usr/lib/libreoffice/program/unopkg.bin
set_pax_flag_on "m" /usr/lib/libreoffice/program/soffice.bin

set_pax_flag_on "m" /usr/lib/valgrind/memcheck-*-linux

# it could be needed to reinstall java before and after, as part of java install process runs java

set_pax_flag_on "m"  /usr/lib/jvm/*/jre/lib/*/*.so  /usr/lib/jvm/*/jre/bin/*  

set_pax_flag_on "m"  /usr/lib/jvm/*/bin/javac

set_pax_flag_on "m"  /usr/lib/icedove/icedove

set +x

# all this problems will be resolved once we have hooks that run this script when other packages are installed
# please contact us #mempo @ irc.oftc.net and ircp2 if you can help with this                 

