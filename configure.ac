dnl ****************************************************************************
dnl Copyright (c) 2001-2004 Michael David Adams.
dnl All rights reserved.
dnl
dnl The complete licensing terms for this software can be found in the file
dnl named "LICENSE" in this software distribution.
dnl ****************************************************************************

dnl The package name.
define(AC_JAS_NAME, jasper)

dnl ************************************************************
dnl Package version information.
dnl ************************************************************

dnl The major, minor, and micro version numbers of the package.
define(AC_JAS_MAJOR_VERSION, 1)
define(AC_JAS_MINOR_VERSION, 900)
define(AC_JAS_MICRO_VERSION, 2)

dnl The package version.
define(AC_JAS_VERSION,
  AC_JAS_MAJOR_VERSION.AC_JAS_MINOR_VERSION.AC_JAS_MICRO_VERSION)

dnl ************************************************************
dnl RPM version information.
dnl ************************************************************

define(AC_JAS_RPM_RELEASE, 1)

dnl ************************************************************
dnl Dynamically-linked library version information (for Libtool).
dnl ************************************************************

dnl Guidelines for updating these definitions:
dnl 1) If the library source code has changed at all since the last update,
dnl then increment REVISION.
dnl 2) If any interfaces have been added, removed, or changed since the last
dnl update, increment CURRENT, and set REVISION to zero.
dnl 3) If any interfaces have been added since the last public release, then
dnl increment AGE.
dnl 4) If any interfaces have been removed since the last public release, then
dnl set AGE to zero.

dnl The most recent interface number that this library implements.
define(AC_JAS_LT_CURRENT, 1)

dnl The implementation number of the current interface.
define(AC_JAS_LT_REVISION, 0)

dnl The difference between the newest and oldest interfaces that this library
dnl implements.  In other words, the library implements all of the interface
dnl numbers in the range from current - age to current.
define(AC_JAS_LT_AGE, 0)

dnl The release number of the library.
define(AC_JAS_LT_RELEASE, AC_JAS_MAJOR_VERSION.AC_JAS_MINOR_VERSION)

dnl ****************************************************************************
dnl NOTHING BELOW THIS LINE SHOULD REQUIRE MODIFICATION (IDEALLY :-)
dnl ****************************************************************************

dnl Initialize autoconf.
AC_INIT(AC_JAS_NAME, AC_JAS_VERSION)
AC_PREREQ(2.59)
AC_CONFIG_SRCDIR(README)
dnl Note: Avoid the temptation to call the auxiliary directory "aux" because
dnl Microsoft Windows will barf if you try to use "aux" as a directory name. :-(
AC_CONFIG_AUX_DIR([ac_aux])
AC_CONFIG_MACRO_DIR([ac_m4])

m4_include([ac_m4/m4_ax_generate_changelog.m4])

AX_GENERATE_CHANGELOG

# Initialize libtool.
LT_INIT([shared static])

# Detect the canonical host and target build environment.
# Note: This precedes automake initialization to avoid warnings.
AC_CANONICAL_BUILD
AC_CANONICAL_HOST
AC_CANONICAL_TARGET

# Initialize automake.
AM_INIT_AUTOMAKE

# Initialize version information parameters.
JAS_MAJOR_VERSION=AC_JAS_MAJOR_VERSION
JAS_MINOR_VERSION=AC_JAS_MINOR_VERSION
JAS_MICRO_VERSION=AC_JAS_MICRO_VERSION
JAS_VERSION=AC_JAS_VERSION
AC_SUBST(JAS_MAJOR_VERSION)
AC_SUBST(JAS_MINOR_VERSION)
AC_SUBST(JAS_MICRO_VERSION)
AC_SUBST(JAS_VERSION)
AC_DEFINE_UNQUOTED(JAS_VERSION, "$JAS_VERSION")
JAS_RPM_RELEASE=AC_JAS_RPM_RELEASE
AC_SUBST(JAS_RPM_RELEASE)

# Initialize version information for libtool.
LT_RELEASE=AC_JAS_LT_RELEASE
LT_CURRENT=AC_JAS_LT_CURRENT
LT_REVISION=AC_JAS_LT_REVISION
LT_AGE=AC_JAS_LT_AGE
AC_SUBST(LT_RELEASE)
AC_SUBST(LT_CURRENT)
AC_SUBST(LT_REVISION)
AC_SUBST(LT_AGE)

# Indicate a configure-based build is being used.
AC_DEFINE(JAS_CONFIGURE)

############################################################
# Configuration header file.
############################################################

AH_TEMPLATE([DEBUG_OVERFLOW], [Debugging overflow detection])
AH_TEMPLATE([JAS_CONFIGURE], [JasPer configure])
AH_TEMPLATE([JAS_VERSION], [JasPer version])
AH_TEMPLATE([DEBUG_MEMALLOC], [Debugging memory allocator])
AH_TEMPLATE([DEBUG], [Extra debugging support])
dnl AH_TEMPLATE([HAVE_VLA], [Have variable length arrays])

AH_TOP([
/* Avoid problems due to multiple inclusion. */
#ifndef JAS_CONFIG_H
#define JAS_CONFIG_H

/* This preprocessor symbol identifies the version of JasPer. */
#undef	JAS_VERSION
/* If configure is being used, this symbol will be defined automatically
  at this point in the configuration header file. */

/* The preprocessor symbol JAS_WIN_MSVC_BUILD should not be defined
  unless the JasPer software is being built under Microsoft Windows
  using Microsoft Visual C. */
#if !defined(JAS_WIN_MSVC_BUILD)
/* A configure-based build is being used. */

])

AH_BOTTOM([
#else
/* A configure-based build is not being used. */
#include <jasper/jas_config2.h>
#endif

#endif
])

############################################################
# Checks for programs.
############################################################

AC_PROG_CC
AC_PROG_INSTALL
# AC_PROG_AWK
# AC_PROG_CPP
# AC_PROG_LN_S

AM_DISABLE_SHARED
AC_PROG_LIBTOOL

AC_PATH_XTRA

dnl AC_MSG_CHECKING(whether variable-length arrays are supported)
dnl AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[]], [[
dnl 	int n; int foo[n];
dnl ]])],[AC_MSG_RESULT(yes); AC_DEFINE(HAVE_VLA)],[AC_MSG_RESULT(no)])

############################################################
# Check for the IJG JPEG library.
############################################################

# Allow the user to explicitly specify whether the JPEG library is present.
AC_ARG_ENABLE(libjpeg,
[  --disable-libjpeg    Force the IJG JPEG library to be ignored],
[
case "${enableval}" in
yes)
	ENABLE_LIBJPEG=yes;;
no)
	ENABLE_LIBJPEG=no;;
*)
	AC_MSG_ERROR(bad value ${enableval} for --disable-libjpeg)
	;;
esac
], ENABLE_LIBJPEG=yes)

HAVE_LIBJPEG=no
if test $ENABLE_LIBJPEG = yes; then
	# Check for the JPEG library.
	AC_CHECK_LIB(jpeg, jpeg_destroy, HAVE_LIBJPEG=yes, HAVE_LIBJPEG=no)
	if test $HAVE_LIBJPEG = yes; then
		LIBS="$LIBS -ljpeg"
	fi
fi
AM_CONDITIONAL(HAVE_LIBJPEG, test $HAVE_LIBJPEG = yes)

############################################################
# Check for OpenGL libraries.
############################################################

AC_ARG_WITH(glut-include-dir,
[  --with-glut-include-dir=DIR    Set GLUT include directory.],
[
	case "$withval" in
	/*)
		CFLAGS="$CFLAGS -I$withval"
		CPPFLAGS="$CPPFLAGS -I$withval";;
	*)
		AC_MSG_ERROR(--with-glut-include-dir requires absolute path)
		;;
	esac
],
)
AC_ARG_WITH(glut-lib-dir,
[  --with-glut-lib-dir=DIR        Set GLUT library directory.],
[
	case "$withval" in
	/*)
		LIBS="$LIBS -L$withval";;
	*)
		AC_MSG_ERROR(--with-glut-lib-dir requires absolute path)
		;;
	esac
],
)

AC_ARG_ENABLE(opengl,
[  --disable-opengl    Disable use of OpenGL. ],
[
	case "$enableval" in
	yes)
		ENABLE_OPENGL=yes;;
	no)
		ENABLE_OPENGL=no;;
	*)
		AC_MSG_ERROR(bad value ${enableval} for --disable-libjpeg)
		;;
	esac
],
ENABLE_OPENGL=yes)

HAVE_OPENGL=no
OPENGL_LIBS=""
if test $ENABLE_OPENGL = yes; then
	if test $HAVE_OPENGL = no; then
		TMPLIBS="-lglut -lGL -lGLU"
		AC_CHECK_LIB(glut, glutInit, [HAVE_OPENGL=yes;
		  OPENGL_LIBS=$TMPLIBS], HAVE_OPENGL=no, $TMPLIBS)
	fi
	AC_CHECK_HEADER(GL/glut.h,, [HAVE_OPENGL=no; OPENGL_LIBS="";])
fi

AM_CONDITIONAL(HAVE_OPENGL, test $HAVE_OPENGL = yes)
AC_SUBST(OPENGL_LIBS)
AC_MSG_CHECKING([whether OpenGL libraries and headers were detected])
AC_MSG_RESULT($HAVE_OPENGL)

############################################################
# Check for other libraries.
############################################################

# Check for the math library.
AC_CHECK_LIB(m, main)

############################################################
# Check for header files.
############################################################

AC_HEADER_STDC
AC_CHECK_HEADERS(fcntl.h limits.h unistd.h stdint.h stdbool.h io.h windows.h sys/types.h sys/time.h stdlib.h stddef.h)

############################################################
# Check for typedefs, structures, and compiler characteristics.
############################################################

AC_C_CONST
AC_C_INLINE
AC_TYPE_SIZE_T
dnl AC_CHECK_TYPE(bool, int)
AC_CHECK_TYPE(uchar, unsigned char)
AC_CHECK_TYPE(ushort, unsigned short)
AC_CHECK_TYPE(uint, unsigned int)
AC_CHECK_TYPE(ulong, unsigned long)
AC_CHECK_TYPE(longlong, long long)
AC_CHECK_TYPE(ulonglong, unsigned long long)
AC_CHECK_TYPE(ssize_t, int)
AC_CHECK_FUNCS(gettimeofday)
AC_CHECK_FUNCS(getrusage)

############################################################
# Checks for library functions.
############################################################

AC_FUNC_VPRINTF

############################################################
# Enable/disable special memory allocator for debugging.
############################################################

AC_ARG_ENABLE(dmalloc,
[  --enable-dmalloc    use special memory allocator for debugging],
[
case "${enableval}" in
yes)
	AC_DEFINE(DEBUG_MEMALLOC)
	;;
no)
	;;
*)
	AC_MSG_ERROR(bad value ${enableval} for --enable-dmalloc)
	;;
esac
], )

############################################################
# Enable/disable extra code for debugging.
############################################################

AC_ARG_ENABLE(debug,
[  --enable-debug    add extra code for debugging],
[
case "${enableval}" in
yes)
	debug=yes
	AC_DEFINE(DEBUG)
	AC_DEFINE(DEBUG_OVERFLOW)
	if test "$GCC" = yes; then
		CFLAGS="-g -O0"
	fi
	;;
no)
	debug=no
	;;
*)
	AC_MSG_ERROR(bad value ${enableval} for --enable-debug)
	;;
esac
], [debug=no])

if test "$GCC" = yes; then
	CFLAGS="$CFLAGS"
	#CFLAGS="$CFLAGS -std=c99"
	#CFLAGS="$CFLAGS -pedantic"
	#CFLAGS="$CFLAGS -pedantic-errors"
	#CFLAGS="$CFLAGS -W -Wall -Wno-long-long -Wformat -Wmissing-prototypes -Wstrict-prototypes"
fi

############################################################
# Extra stuff for research purposes.
############################################################

AC_ARG_ENABLE(special0,
[  --enable-special0    enable something],
[
case "${enableval}" in
yes)
	if test "$GCC" = yes; then
		CFLAGS="-g -O0"
	fi
	;;
no)
	;;
*)
	AC_MSG_ERROR(bad value ${enableval} for --enable-special0)
	;;
esac
], [])

if test -n "$EXTRACFLAGS"; then
	CFLAGS="$CFLAGS $EXTRACFLAGS"
fi

############################################################
# Generate the configuration header file.
############################################################

AC_CONFIG_HEADERS([src/libjasper/include/jasper/jas_config.h])

############################################################
# Generate the makefiles.
############################################################

AC_CONFIG_FILES([
Makefile
src/Makefile
src/appl/Makefile
src/libjasper/Makefile
src/libjasper/base/Makefile
src/libjasper/bmp/Makefile
src/libjasper/include/Makefile
src/libjasper/include/jasper/Makefile
src/libjasper/jp2/Makefile
src/libjasper/jpc/Makefile
src/libjasper/jpg/Makefile
src/libjasper/mif/Makefile
src/libjasper/pgx/Makefile
src/libjasper/pnm/Makefile
src/libjasper/ras/Makefile
src/msvc/Makefile
pkgconfig/Makefile
pkgconfig/jasper.pc
jasper.spec
])
AC_OUTPUT

