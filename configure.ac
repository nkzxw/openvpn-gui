dnl  OpenVPN-GUI -- A Windows GUI for OpenVPN.
dnl
dnl  Copyright (C) 2004 Mathias Sundman <mathias@nilings.se>
dnl                2010 Heiko Hund <heikoh@users.sf.net>
dnl
dnl  This program is free software; you can redistribute it and/or modify
dnl  it under the terms of the GNU General Public License as published by
dnl  the Free Software Foundation; either version 2 of the License, or
dnl  (at your option) any later version.
dnl
dnl  This program is distributed in the hope that it will be useful,
dnl  but WITHOUT ANY WARRANTY; without even the implied warranty of
dnl  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
dnl  GNU General Public License for more details.
dnl
dnl  You should have received a copy of the GNU General Public License
dnl  along with this program (see the file COPYING included with this
dnl  distribution); if not, write to the Free Software Foundation, Inc.,
dnl  59 Temple Place, Suite 330, Boston, MA  02111-1307  USA

AC_PREREQ(2.59)
define([_GUI_VERSION], [11])
AC_INIT([OpenVPN GUI],[_GUI_VERSION],[openvpn-devel@lists.sourceforge.net],[openvpn-gui],[https://github.com/openvpn/openvpn-gui/])
AC_DEFINE([PACKAGE_VERSION_RESOURCE], [_GUI_VERSION,0,0,0], [Version in windows resource format])
AC_CONFIG_AUX_DIR([.])
AM_CONFIG_HEADER([config.h])
AC_CONFIG_SRCDIR([main.h])
AM_INIT_AUTOMAKE
AC_CANONICAL_HOST
AC_USE_SYSTEM_EXTENSIONS
AC_PROG_CC_C99
AC_CHECK_TOOL([WINDRES], [windres])

AC_ARG_ENABLE(
	[distonly],
	[AS_HELP_STRING([--enable-distonly], [enable distribute only mode @<:@default=no@:>@])],
	,
	[enable_distonly="no"]
)

AC_ARG_VAR([MAX_CONFIGS], [specify the maximum number of configs @<:@default=50@:>@])
if test -z "$MAX_CONFIGS"; then
	MAX_CONFIGS=50
fi
AC_DEFINE_UNQUOTED([MAX_CONFIGS], [$MAX_CONFIGS], [Maximum number of config files supported.])

AC_ARG_VAR([ARCH], [override the CPU architecture for the NSIS installer])
test "x$ARCH" = "x" && ARCH=${host%%-*}

case "$host" in
	*-mingw*)
		CPPFLAGS="${CPPFLAGS} -DWIN32_LEAN_AND_MEAN"
		CPPFLAGS="${CPPFLAGS} -D_WIN32_WINNT=NTDDI_WINXP"
		;;
	*)
		test "${enable_distonly}" = "no" && AC_MSG_ERROR([This project is supported for windows only.])
		;;
esac

PKG_PROG_PKG_CONFIG
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET

AC_ARG_VAR([MAKENSIS], [define the path to the makensis tool])
if test "x$MAKENSIS" = "x"; then
	AC_PATH_PROG([MAKENSIS], [makensis])
fi
AM_CONDITIONAL([HAVE_NSIS], [test "x$MAKENSIS" != "x"])

PKG_CHECK_MODULES(
	[OPENSSL_CRYPTO],
	[libcrypto >= 0.9.6],
	,
	[AC_CHECK_LIB(
		[crypto],
		[RSA_new],
		[OPENSSL_CRYPTO_LIBS="-lcrypto"],
		[test "${enable_distonly}" = "no" && AC_MSG_ERROR([Cannot find OpenSSL library])]
	)]
)

test "${GCC}" = "yes" && CFLAGS="${CFLAGS} -pedantic -Wall -Wextra"

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
