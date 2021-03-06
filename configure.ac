#                                                       -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

# Copyright (C) 1992, 1993, 1994, 1995, 1999, 2000, 2001, 2002, 2003
# Free Software Foundation, Inc.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2, or (at your option)
# any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA
# 02111-1307, USA.

# We need AC_CONFIG_TESTDIR.
AC_PREREQ([2.57])

dnl  Since this is a Starlink-customised version of autoconf, change the
dnl  package name and bug-reporting address.  It would be good to change
dnl  the version number, too, but that would require surgery on macro
dnl  lib/m4sugar/m4sugar.m4:m4_version_prereq.
dnl
dnl  Include the TARNAME field, not because we want to distribute
dnl  this package, but so that autoconf uses this when building some
dnl  installation locations rather than the "starlink-autoconf" it
dnl  would otherwise construct.
AC_INIT([Starlink Autoconf], [2.59], [starlink@jiscmail.ac.uk], autoconf)
dnl AC_INIT([GNU Autoconf], [2.59], [bug-autoconf@gnu.org])
AC_SUBST([PACKAGE_NAME])dnl
AC_CONFIG_SRCDIR([ChangeLog])

# Set the STAR_MANIFEST_DIR variable, defaulting it from the environment.
# This causes the `install-manifest' target to be included in Makefile.in
: ${STAR_MANIFEST_DIR='$(prefix)/manifests'}
AC_SUBST(STAR_MANIFEST_DIR)

AC_CONFIG_AUX_DIR([config])
AC_CONFIG_FILES([config/Makefile])

AM_INIT_AUTOMAKE([check-news 1.7.8 dist-bzip2 readme-alpha])

# Initialize the test suite and build position independent wrappers.
AC_CONFIG_TESTDIR([tests])
AC_CONFIG_FILES([tests/Makefile tests/atlocal])
AC_CONFIG_FILES([tests/autoconf:tests/wrapper.in],
                [chmod +x tests/autoconf])
AC_CONFIG_FILES([tests/autoheader:tests/wrapper.in],
                [chmod +x tests/autoheader])
AC_CONFIG_FILES([tests/autom4te:tests/wrapper.in],
                [chmod +x tests/autom4te])
AC_CONFIG_FILES([tests/autoreconf:tests/wrapper.in],
                [chmod +x tests/autoreconf])
AC_CONFIG_FILES([tests/autoscan:tests/wrapper.in],
                [chmod +x tests/autoscan])
AC_CONFIG_FILES([tests/autoupdate:tests/wrapper.in],
                [chmod +x tests/autoupdate])
AC_CONFIG_FILES([tests/ifnames:tests/wrapper.in],
                [chmod +x tests/ifnames])
AC_PATH_PROG([EXPR], [expr])


## ---- ##
## M4.  ##
## ---- ##

# We use a path for GNU m4 so even if users have another m4 first in
# their path, the installer can configure with a path that has GNU m4
# first and get that path embedded in the installed autoconf and
# autoheader scripts.
AC_PROG_GNU_M4
if test x"$ac_cv_prog_gnu_m4" != xyes; then
  AC_MSG_ERROR([GNU M4 1.4 is required])
fi

## ----------- ##
## Man pages.  ##
## ----------- ##

AC_CONFIG_FILES(man/Makefile)
AM_MISSING_PROG(HELP2MAN, help2man)


## ------ ##
## Perl.  ##
## ------ ##

# We use a path for perl so the #! line in autoscan will work.
AC_PATH_PROG([PERL], perl, no)
AC_SUBST([PERL])dnl
if test "$PERL" = no; then
  AC_MSG_ERROR([perl is not found])
fi
$PERL -e 'require 5.005_03;' || {
   AC_MSG_ERROR([Perl 5.005_03 or better is required])
}


## ------- ##
## Emacs.  ##
## ------- ##

AC_CONFIG_FILES([lib/emacs/Makefile])
TEST_EMACS=$EMACS
test x"$TEST_EMACS" = xt && TEST_EMACS=
AC_CHECK_PROGS([TEST_EMACS], [emacs xemacs], [no])

# autoconf-mode.el and autotest-mode.el do not work with older version of
# Emacs (i.e. 18.x.x).  During byte-compilation, Emacs complains:
# "Variable (broken nil) seen on pass 2 of byte compiler but not pass 1"
# We detect this problem here.
AS_IF([test "$TEST_EMACS" != no],
  [AC_CACHE_CHECK([whether $TEST_EMACS is sufficiently recent],
    [ac_cv_prog_emacs_ok],
    [# Note: The quoted "EOF" is intentional.  It protects the ` in the text.
    cat >conftest.el << "EOF"
      (defvar broken)
      (defun broken-mode () (setq broken-defaults `(broken nil)))
EOF
    AS_IF([AC_RUN_LOG(
      [$TEST_EMACS -batch -q -f batch-byte-compile conftest.el 1>&2])],
      [ac_cv_prog_emacs_ok=yes], [ac_cv_prog_emacs_ok=no])
    rm -f conftest.el conftest.elc])
  AS_IF([test $ac_cv_prog_emacs_ok = no], [TEST_EMACS=no])])

AC_SUBST([EMACS], [$TEST_EMACS])

AM_PATH_LISPDIR


## ------------ ##
## Conclusion.  ##
## ------------ ##

AC_CONFIG_FILES([Makefile doc/Makefile
                 lib/Makefile lib/Autom4te/Makefile lib/autoscan/Makefile
                 lib/m4sugar/Makefile
                 lib/autoconf/Makefile lib/autotest/Makefile
                 bin/Makefile])

dnl  Starlink-specific bookkeeping
AC_CONFIG_FILES([component.xml])

AC_OUTPUT
# Report the state of this version of Autoconf if this is a beta.
m4_bmatch(m4_defn([AC_PACKAGE_VERSION]),  [[a-z]],
[    cat <<EOF

You are about to use an experimental version of Autoconf.  Be sure to
read the relevant mailing lists, most importantly <autoconf@gnu.org>.

Below you will find information on the status of this version of Autoconf.


EOF
    sed -n '/^\* Status/,$p' $srcdir/BUGS
])
