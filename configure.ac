dnl
dnl BulkRays Copyright (C) 2012-2020 Jean-Daniel Pauget
dnl A whole set of http tcp sessions
dnl
dnl jdbulkrayed@disjunkt.com  -  http://bulkrays.disjunkt.com/

dnl This file is part of BulkRays
dnl 
dnl BulkRays is free software; you can redistribute it and/or modify
dnl it under the terms of the GNU General Public License as published by
dnl the Free Software Foundation; either version 2 of the License, or
dnl (at your option) any later version.
dnl 
dnl BulkRays is distributed in the hope that it will be useful,
dnl but WITHOUT ANY WARRANTY; without even the implied warranty of
dnl MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
dnl GNU General Public License for more details.
dnl 
dnl You should have received a copy of the GNU General Public License
dnl along with BulkRays; if not, write to the Free Software
dnl Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
dnl 
dnl you can also try the web at http://www.gnu.org/
dnl 
dnl
dnl bulkrays@nekodune.com  -  http://bulkrays.nekodune.com/

dnl Process this file with autoconf to produce a configure script.
AC_INIT([simplefmap], [0.0.6], [bulkrays@nekodune.com])
AC_CONFIG_SRCDIR([./simplefmap.hcpp])


dnl Detect the canonical host and target build environment
AC_CANONICAL_HOST
AC_CANONICAL_TARGET


dnl quieten some complains from missing files
AM_INIT_AUTOMAKE([foreign])

dnl the magic of additionnal local m4 ?
AC_CONFIG_MACRO_DIR([m4])

dnl autoheader
AM_CONFIG_HEADER(config.h)

dnl Check for tools

AC_PROG_CC
AC_PROG_CXX
AC_LIBTOOL_WIN32_DLL
AM_PROG_LIBTOOL

dnl Check for compiler environment

AC_C_CONST

dnl JD's standard minimum CFLAG
dnl CHANGED for JD's ENterGroup default nasty warnings
CFLAGS="$CFLAGS -O2 -Wall -Wcast-qual -Wwrite-strings -Wstrict-prototypes -Wmissing-prototypes"
dnl CXXFLAGS="$CXXFLAGS -O2 -Wall -Woverloaded-virtual -Wcast-qual -Wwrite-strings -Wstrict-prototypes -Wmissing-prototypes -Wredundant-decls -Winline"
CXXFLAGS="$CXXFLAGS -O2 -Wall -Woverloaded-virtual -Wcast-qual -Wwrite-strings -Wredundant-decls"

dnl offer a bypass for qiconn lib
AC_ARG_ENABLE(libqiconn-path,
[  --enable-libqiconn-path define the path for libqiconn and includes],
    qiconnpath=$enableval, qiconnpath="")
if test "x${qiconnpath}" = "x"; then
:
else
    INCPATH="$INCPATH -I${qiconnpath}/include"
    QICONNLDFLAGS="-L${qiconnpath}"
fi

dnl allow static linking to qiconn
AC_ARG_ENABLE(qiconn-static,
[  --enable-qiconn-static  link with static qiconn libes],
    qiconnstatic=$enableval, qiconnstatic="no")
if test "x$qiconnstatic" = "xyes"; then
    QICONNLDFLAGS=""
    QICONNLDLIB="${qiconnpath}/libqiconn.a"
else
    QICONNLDFLAGS="$QICONNLDFLAGS -lqiconn"
    QICONNLDLIB=""
fi

dnl check for qiconn include and lib
AC_MSG_CHECKING(for qiconn lib)
have_qiconn=no
SAVECXXFLAGS="$CXXFLAGS"
CXXFLAGS="$INCPATH $SAVECXXFLAGS"
AC_LANG(C++)
## AC_TRY_COMPILE([
## #include <qiconn/qiconn.h>
## ],[
##     qiconn::ConnectionPool cp;
##     cp.add_signal_handler (13);
## ],[
## have_qiconn=yes
## ])
if test x$have_qiconn = xno; then
    if test "${prefix}" = "NONE"  ; then
	TESTPREFIX="/usr/local"
    else
	TESTPREFIX="${prefix}"
    fi
    for P_INC in "${TESTPREFIX}" /usr/local /usr
    do
	if test x$have_qiconn = xno; then
	    echo -n "  ($P_INC :"
	    CXXFLAGS="$INCPATH -I$P_INC/include $SAVECXXFLAGS"
	    AC_TRY_COMPILE([
#include <qiconn/qiconn.h>
void testqiconn (void) {
    qiconn::ConnectionPool cp;
    cp.add_signal_handler (13);
}
	    ],[
	    ],[
	    have_qiconn=yes
	    ])
	    if test x$have_qiconn = xyes; then
		INCPATH="$INCPATH -I$P_INC/include"
		if test "x$qiconnstatic" = "xyes"; then
		    QICONNLDFLAGS=""
		    QICONNLDLIB="${P_INC}/lib/libqiconn.a"
		else
		    QICONNLDFLAGS="-L${P_INC}/lib -lqiconn"
		    QICONNLDLIB=""
		fi

		echo -n " yes.)"
	    else
		echo -n " no.)"
	    fi
	fi
    done
    echo -n " : "
fi
CXXFLAGS="$SAVECXXFLAGS"
AC_MSG_RESULT($have_qiconn)
if test x$have_qiconn = xyes; then
    :
else
    AC_MSG_ERROR(Unable to find qiconn headers)
fi
AC_SUBST(QICONNLDFLAGS)
AC_SUBST(QICONNLDLIB)
dnl end of check for qiconn lib --------------------------------------------------



dnl check for mhash include and lib
AC_MSG_CHECKING(for mhash lib)
have_mhash=no
SAVECXXFLAGS="$CXXFLAGS"
CXXFLAGS="$INCPATH $SAVECXXFLAGS"
AC_LANG(C++)
## AC_TRY_COMPILE([
## #include <mhash.h>
## void testmhash (void) {
## 
##     keygenid algorithm = KEYGEN_S2K_SALTED;
##     mhash_keygen_uses_salt(algorithm);
## }
## ],[
## ],[
## have_mhash=yes
## ])
if test x$have_mhash = xno; then
    if test "${prefix}" = "NONE"  ; then
	TESTPREFIX="/usr/local"
    else
	TESTPREFIX="${prefix}"
    fi
    for P_INC in "${TESTPREFIX}" /opt/local /usr/local /usr
    do
	if test x$have_mhash = xno; then
	    echo -n "  ($P_INC :"
	    CXXFLAGS="$INCPATH -I$P_INC/include $SAVECXXFLAGS"
	    AC_TRY_COMPILE([
#include <mhash.h>
void testmhash (void) {

    keygenid algorithm = KEYGEN_S2K_SALTED;
    mhash_keygen_uses_salt(algorithm);
}
	    ],[
	    ],[
	    have_mhash=yes
	    ])
	    if test x$have_mhash = xyes; then
		LIBMHASH_CFLAGS="-I$P_INC/include"
		LIBMHASH_LDFLAGS="-L$P_INC/lib -lmhash"
		echo -n " yes.)"
	    else
		echo -n " no.)"
	    fi
	fi
    done
    echo -n " : "
fi
CXXFLAGS="$SAVECXXFLAGS"
AC_MSG_RESULT($have_mhash)
if test x$have_mhash = xyes; then
    :
else
    AC_MSG_ERROR(Unable to find mhash headers)
fi
AC_SUBST(LOCALINCPATH)
AC_SUBST(LIBMHASH_CFLAGS)
AC_SUBST(LIBMHASH_LDFLAGS)
dnl end of check for mhash lib --------------------------------------------------


dnl offer a bypass for bulkrays lib
AC_ARG_ENABLE(libbulkrays-path,
[  --enable-libbulkrays-path define the path for libbulkrays and includes],
    bulkrayspath=$enableval, bulkrayspath="")
if test "x${bulkrayspath}" = "x"; then
:
else
    INCPATH="$INCPATH -I${bulkrayspath}/include"
    BULKRAYSLDFLAGS="-L${bulkrayspath}"
fi

dnl allow static linking to bulkrays
AC_ARG_ENABLE(bulkrays-static,
[  --enable-bulkrays-static  link with static bulkrays libes],
    bulkraysstatic=$enableval, bulkraysstatic="no")
if test "x$bulkraysstatic" = "xyes"; then
    BULKRAYSLDFLAGS=""
    BULKRAYSLDLIB="${bulkrayspath}/libbulkrays.a"
else
    BULKRAYSLDFLAGS="$BULKRAYSLDFLAGS -lbulkrays"
    BULKRAYSLDLIB=""
fi

dnl check for bulkrays include and lib
AC_MSG_CHECKING(for bulkrays lib)
have_bulkrays=no
SAVECXXFLAGS="$CXXFLAGS"
CXXFLAGS="$INCPATH $SAVECXXFLAGS"
AC_LANG(C++)
## AC_TRY_COMPILE([
## #include <bulkrays/bulkrays.h>
## ],[
##     bulkrays::ConnectionPool cp;
##     cp.add_signal_handler (13);
## ],[
## have_bulkrays=yes
## ])
if test x$have_bulkrays = xno; then
    if test "${prefix}" = "NONE"  ; then
	TESTPREFIX="/usr/local"
    else
	TESTPREFIX="${prefix}"
    fi
    for P_INC in "${TESTPREFIX}" /usr/local /usr
    do
	if test x$have_bulkrays = xno; then
	    echo -n "  ($P_INC :"
	    CXXFLAGS="$INCPATH -I$P_INC/include $SAVECXXFLAGS"
	    AC_TRY_COMPILE([
#include <bulkrays/bulkrays.h>
namespace bulkrays {
    using namespace std;
    int bootstrap_global (bulkrays::BSOperation op) {
        int i = 0;
	return i;
    }
} // namespace bulkrays
	    ],[
	    ],[
	    have_bulkrays=yes
	    ])
	    if test x$have_bulkrays = xyes; then
		INCPATH="$INCPATH -I$P_INC/include"
		if test "x$bulkraysstatic" = "xyes"; then
		    BULKRAYSLDFLAGS=""
		    BULKRAYSLDLIB="${P_INC}/lib/libbulkrays.a"
		else
		    BULKRAYSLDFLAGS="-L${P_INC}/lib -lbulkrays"
		    BULKRAYSLDLIB=""
		fi

		echo -n " yes.)"
	    else
		echo -n " no.)"
	    fi
	fi
    done
    echo -n " : "
fi
CXXFLAGS="$SAVECXXFLAGS"
AC_MSG_RESULT($have_bulkrays)
if test x$have_bulkrays = xyes; then
    :
else
    AC_MSG_ERROR(Unable to find bulkrays headers)
fi
AC_SUBST(BULKRAYSLDFLAGS)
AC_SUBST(BULKRAYSLDLIB)
dnl end of check for bulkrays lib --------------------------------------------------

dnl dnl badly look for bulkrays includes ...
dnl AC_MSG_CHECKING(for bulkrays headers)
dnl if test -f ./bulkrays/bulkrays.h ; then
dnl     LOCALINCPATH="-I."
dnl     AC_MSG_RESULT(found)
dnl else
dnl     AC_MSG_ERROR(Unable to find bulkrays hearders)
dnl fi
dnl 
dnl dnl badly look for bulkrays includes ...
dnl AC_MSG_CHECKING(for bulkrays headers)
dnl if test -f ./bulkrays/bulkrays.h ; then
dnl     INCPATH="-I. $INCPATH"
dnl     AC_MSG_RESULT(found)
dnl else
dnl     AC_MSG_ERROR(Unable to find bulkrays hearders)
dnl fi








dnl check for mac-osX file scheme
case "$target" in
    *-apple-darwin*)
	AC_DEFINE(OSXFILESCHEME, 1, a macosX file scheme flavour is used)
	;;
    *)
	AC_DEFINE(OSXFILESCHEME, 0, a regular unix file scheme flavour is used)
	;;
esac

dnl check for mac-osX gcc stl variant
case "$target" in
    *-apple-darwin*)
	CXXFLAGS="$CXXFLAGS -stdlib=libstdc++"
	AC_MSG_RESULT([stdlib variant : -stdlib=libstdc++])
	;;
    *)
	:
	;;
esac


dnl concatenation of include dirs
CFLAGS="$INCPATH $CFLAGS"
CXXFLAGS="$INCPATH $CXXFLAGS"
dnl Finally create all the generated files
AC_OUTPUT([
	Makefile
])


