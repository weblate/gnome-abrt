AC_INIT([gnome-abrt],
        m4_esyscmd([cat ./gnome-abrt-version]),
        [crash-catcher@fedorahosted.org])

AC_CONFIG_MACRO_DIR([m4])
AM_INIT_AUTOMAKE([-Wall foreign])
AM_MAINTAINER_MODE
# Support silent build rules. Disable by either passing --disable-silent-rules
# to ./configure or passing V=1 to make
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES])
# The 'yes' argument is undocumented but it is common knowledge that it works.
AM_SILENT_RULES([yes])

AC_DISABLE_STATIC
AC_PROG_CC
AC_GNU_SOURCE
AM_PROG_AR
AC_PROG_LIBTOOL
AC_PROG_LN_S

CFLAGS="$CFLAGS -std=c99 -pedantic -Wall -Wwrite-strings -Wextra"

AM_PATH_PYTHON([3.4])
#AX_PYTHON_MODULE([rpm], [1])
#AX_PYTHON_MODULE([rpmUtils.miscutils], [1])

AC_PATH_PROGS(BASH, sh bash)

# Manual pages.
AC_PATH_PROG([ASCIIDOC], [asciidoc], [no])
[if test "$ASCIIDOC" = "no"]
[then]
    [echo "The asciidoc program was not found in the search path. Please ensure"]
    [echo "that it is installed and its directory is included in the search path."]
    [echo "Then run configure again before attempting to build gnome-abrt."]
    [exit 1]
[fi]

# Manual pages.
AC_PATH_PROG([XMLTO], [xmlto], [no])
[if test "$XMLTO" = "no"]
[then]
    [echo "The xmlto program was not found in the search path. Please ensure"]
    [echo "that it is installed and its directory is included in the search path."]
    [echo "Then run configure again before attempting to build gnome-abrt."]
    [exit 1]
[fi]

# make check
AC_ARG_WITH(nopylint,
        AS_HELP_STRING([--with-nopylint],[disable pylint check (default is NO)]),
        [], [with_nopylint=no])

[if test "$with_nopylint" = "no"]
[then]
    AC_PATH_PROGS([PYLINT], [python3-pylint pylint-3], [no])
    [if test "$PYLINT" = "no"]
    [then]
        [echo "The pylint program was not found in the search path. Please ensure"]
        [echo "that it is installed and its directory is included in the search path."]
        [echo "Then run configure again before attempting to build gnome-abrt."]
        [exit 1]
    [fi]
    AM_CONDITIONAL(USE_PYLINT, true)
[else]
    AM_CONDITIONAL(USE_PYLINT, false)
[fi]


dnl ****** INTERNATIONALIZATION **********************

GETTEXT_PACKAGE=gnome-abrt
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED(GETTEXT_PACKAGE, "$GETTEXT_PACKAGE", [Package name for gettext])

AM_GNU_GETTEXT([external])
AM_GNU_GETTEXT_VERSION([0.19.6])

dnl ****** END ****************************************

PYTHON_CFLAGS=`python3-config --cflags 2> /dev/null`
PYTHON_LIBS=`python3-config --libs 2> /dev/null`
GNOME_ABRT_VERSION=`cat ./gnome-abrt-version`

AC_SUBST(PYTHON_CFLAGS)
AC_SUBST(GNOME_ABRT_VERSION)

PKG_CHECK_MODULES([GTK], [gtk+-3.0 >= 3.10.0])
PKG_CHECK_MODULES([LIBREPORT_GTK], [libreport-gtk > 2.0.19-3])
PKG_CHECK_MODULES([ABRT_GUI], [abrt_gui > 2.1.6])
PKG_CHECK_MODULES([PYGOBJECT3], [pygobject-3.0 >= 3.29.1])
PKG_CHECK_MODULES([PYTHON], [python3])
PKG_CHECK_MODULES([X11], [x11])

GLIB_COMPILE_RESOURCES=`pkg-config --variable glib_compile_resources gio-2.0`
AC_SUBST([GLIB_COMPILE_RESOURCES])

AC_CONFIG_FILES([
    gnome-abrt.spec
    Makefile
    data/Makefile
    doc/Makefile
    icons/Makefile
    src/Makefile
    src/gnome_abrt/Makefile
    src/gnome_abrt/wrappers/Makefile
    src/gnome_abrt/url/Makefile
    po/Makefile.in
])

AC_OUTPUT
