dnl                                               -*- Autoconf -*-
dnl Process this file with autoconf to produce a configure script.

AC_INIT([fdk-aac-dabplus], [0.6.0], [http://opendigitalradio.org/])
AC_CONFIG_AUX_DIR(.)
AC_CONFIG_MACRO_DIR([m4])
AM_INIT_AUTOMAKE([tar-ustar foreign])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])
AC_CONFIG_HEADER([config.h])

dnl Checks for programs.
AC_PROG_CC
AC_PROG_CXX
AC_PROG_LIBTOOL
LT_INIT

AC_CHECK_LIB([m], [sin])

AX_BOOST_BASE([1.41.0], [], AC_MSG_ERROR([BOOST 1.41 or later is required]))
AC_CHECK_LIB([boost_system], [main], [], [AC_MSG_ERROR([library boost_system is missing])])
AC_CHECK_LIB([boost_thread], [main], [], [AC_MSG_ERROR([library boost_thread is missing])])

AC_CHECK_LIB([rt], [clock_gettime], [], [AC_MSG_ERROR([library rt is missing])])

AM_PATH_ALSA(1.0.25)

# Check for JACK
AC_ARG_ENABLE([jack],
        AS_HELP_STRING([--enable-jack], [Enable JACK input]))

AS_IF([test "x$enable_jack" = "xyes"],
       AC_CHECK_LIB(jack, jack_client_open, [],
                    [AC_MSG_ERROR([JACK is required])]))
AS_IF([test "x$enable_jack" = "xyes"],
       AC_DEFINE(HAVE_JACK, [1], [Define if JACK input is enabled]))

# Link against jack
AM_CONDITIONAL([HAVE_JACK], [ test "x$enable_jack" = "xyes" ])

# fdk-aac-dabplus-zmq needs ZeroMQ
AC_CHECK_LIB(zmq, zmq_init, , AC_MSG_ERROR(ZeroMQ libzmq is required))

if pkg-config MagickWand; then
    MAGICKWAND_CFLAGS=`pkg-config MagickWand --cflags`
    MAGICKWAND_LDADD=`pkg-config MagickWand --libs`
    AC_SUBST(MAGICKWAND_CFLAGS)
    AC_SUBST(MAGICKWAND_LDADD)
else
    AC_MSG_ERROR(MagickWand not found)
fi

dnl soname version to use
dnl goes by ‘current[:revision[:age]]’ with the soname ending up as
dnl current.age.revision
FDK_AAC_VERSION=0:4:0

AS_IF([test x$enable_shared = xyes], [LIBS_PRIVATE=$LIBS], [LIBS_PUBLIC=$LIBS])
AC_SUBST(FDK_AAC_VERSION)
AC_SUBST(LIBS_PUBLIC)
AC_SUBST(LIBS_PRIVATE)

AM_CONDITIONAL([IS_GIT_REPO], [test -d '.git'])

AC_CONFIG_FILES([Makefile
                 fdk-aac-dabplus.pc])
AC_OUTPUT
