dnl Process this file with autoconf to produce a configure script.

AM_CONFIG_HEADER(config.h)

AC_INIT(xsensors, 0.80)
AC_CONFIG_SRCDIR(src/main.c)
AM_INIT_AUTOMAKE

AC_PREREQ([2.50])

AM_MAINTAINER_MODE

AC_PROG_CC
AC_ISC_POSIX
AC_HEADER_STDC

dnl Checks for the sensors library.
AC_CHECK_LIB(sensors, sensors_init,[],
	AC_MSG_ERROR(You need the sensors library))

AC_CHECK_HEADERS([sensors/sensors.h sensors/error.h])

AM_PATH_GLIB_2_0(2.0.0,,AC_MSG_ERROR([
*** GLib 2.0 is required to build xsensors; please make sure you have the GLib
*** development headers installed. The latest version of GLib is
*** always available at http://www.gtk.org/.]))

AC_ARG_WITH([gtk2], [AS_HELP_STRING([--with-gtk2],
            [Force GTK2 GUI, even if GTK3 is available @<:@default=check@:>@])],
            [], [with_gtk2=no])

if [ test "x$with_gtk2" == xno ]; then
    PKG_CHECK_MODULES([GTK],[gtk+-3.0],foundgtk3=true,foundgtk3=false)
else
    echo "Using GTK2..."
    foundgtk3=false
fi

if [ $foundgtk3 ]; then
	XSENSORS_CFLAGS="`pkg-config --cflags gtk+-3.0` -Werror -Wall"
	XSENSORS_LIBS="`pkg-config --libs gtk+-3.0`"
else
    if [ test "x$with_gtk2" == xno ]; then
        echo "GTK3 is not installed, checking for GTK2 instead..."
    fi
	AM_PATH_GTK_2_0(2.8.0,,AC_MSG_ERROR([
*** GTK+ is required to build xsensors; please make sure you have the GTK+
*** development headers installed. The latest version of GTK+ is
*** always available at http://www.gtk.org/.]))
	XSENSORS_CFLAGS="`pkg-config --cflags gtk+-2.0` -Werror -Wall"
	XSENSORS_LIBS="`pkg-config --libs gtk+-2.0`"
fi

AC_SUBST(XSENSORS_CFLAGS)
AC_SUBST(XSENSORS_LIBS)

AC_DEFINE_UNQUOTED(XSENSORS_ICONDIR, "${prefix}/share/pixmaps")

AC_SUBST(CFLAGS)
AC_SUBST(CPPFLAGS)
AC_SUBST(LDFLAGS)

AC_OUTPUT([
	Makefile
	xdg/Makefile
	src/Makefile
])
