#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT(nfqfilter, 0.1, max1976@mail.ru)

AM_INIT_AUTOMAKE

AC_CONFIG_SRCDIR([src/main.cpp])
AC_CONFIG_HEADERS([include/config.h])

AC_LANG([C++])
AC_LANG_PUSH([C++])

# store current user given compiler flags to avoid default setup via AC_PROG_CXX
OLD_CXXFLAGS=$CXXFLAGS
OLD_CFLAGS=$CFLAGS

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC

CXXFLAGS=$OLD_CXXFLAGS
CFLAGS=$OLD_CFLAGS

CFLAGS="$CFLAGS --pedantic -Wall -O2"

AC_ARG_ENABLE(debug,
  AS_HELP_STRING(
    [--enable-debug],
    [enable debugging, default: no]),
    [case "${enableval}" in
      yes) debug=true ;;
      no)  debug=false ;;
      *)   AC_MSG_ERROR([bad value ${enableval} for --enable-debug]) ;;
    esac],
    [debug=false])

AC_MSG_CHECKING([for debug enabled])


if test x"$debug" = x"true"; then
    CXXFLAGS="$CXXFLAGS -std=c++0x -O2 -g -Wall"
else
    CXXFLAGS="$CXXFLAGS -std=c++0x -O2"
fi

AC_COMPILE_IFELSE([AC_LANG_SOURCE(
  [[template <typename T>
    struct check
    {
      static_assert(sizeof(int) <= sizeof(T), "not big enough");
    };

    typedef check<check<bool>> right_angle_brackets;

    int a;
    decltype(a) b;

    typedef check<int> check_type;
    check_type c;
    check_type&& cr = static_cast<check_type&&>(c);]])],,
  AC_MSG_FAILURE(['$CXX $CXXFLAGS' does not accept ISO C++11]))




# Checks for libraries.

# Checks for header files.
AC_CHECK_HEADERS([netinet/in.h stdint.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_CHECK_HEADER_STDBOOL

# Checks for library functions.
AC_FUNC_ERROR_AT_LINE
AC_CHECK_FUNCS([strerror])

# Check for methods in library and check for header files
AC_CHECK_HEADERS([Poco/Foundation.h Poco/Net/HTTPCookie.h Poco/Util/Timer.h],
   [],
   AC_MSG_ERROR([Poco include files not found.])
)

AC_CHECK_LIB([PocoFoundation],[main],[HAVE_POCOFOUNDATION=1],AC_MSG_ERROR([PocoFoundation library not found.]))
if test "$HAVE_POCOFOUNDATION"; then
     save_libs="${LIBS}"
     LIBS="-lPocoFoundation"
     AC_LINK_IFELSE(
         [AC_LANG_PROGRAM(
             [#include "Poco/UnicodeConverter.h"],
             [std::wstring wstr; Poco::UnicodeConverter::toUTF16("hello", wstr);]
         )],
         [LIBS="$LIBS $save_libs"],
         [AC_MSG_ERROR([linking with PocoFoundation failed.])]
     )
fi

AC_CHECK_LIB([PocoUtil],[main],[HAVE_POCOUTIL=1],AC_MSG_ERROR([PocoUtil library not found.]))
 if test "$HAVE_POCOUTIL"; then
     save_libs="${LIBS}"
     LIBS="-lPocoUtil"
     AC_LINK_IFELSE(
         [AC_LANG_PROGRAM(
             [#include "Poco/Util/Option.h"],
             [Poco::Util::Option();]
         )],
         [LIBS="$LIBS $save_libs"],
         [AC_MSG_ERROR([linking with PocoUtil failed.])]
     )
fi

AC_CHECK_LIB([PocoNet],[main],[HAVE_POCONET=1],AC_MSG_ERROR([PocoNet library not found.]))
if test "$HAVE_POCONET"; then
     save_libs="${LIBS}"
     LIBS="-lPocoNet"
     AC_LINK_IFELSE(
         [AC_LANG_PROGRAM(
             [#include "Poco/Net/HTTPClientSession.h"],
             [Poco::Net::HTTPClientSession();]
         )],
         [LIBS="$LIBS $save_libs"],
         [AC_MSG_ERROR([linking with PocoNET failed.])]
     )
fi

AC_CHECK_LIB([netfilter_queue], [nfq_open],,AC_MSG_ERROR([Netfilter_queue library not found!]))
AC_CHECK_LIB([nfnetlink], [nfnl_fd],,AC_MSG_ERROR([Nfnetlink library not found!]))

AC_CHECK_LIB([ndpi], [ndpi_init_detection_module],,AC_MSG_ERROR([nDPI library not found!]))

AC_OUTPUT(Makefile src/Makefile include/Makefile)
