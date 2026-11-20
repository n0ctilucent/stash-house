AC_PREREQ([2.69])
AC_INIT([stash-house], [2.0.0], [n0ctilucent@users.noreply.github.com], [https://github.com/n0ctilucent/stash-house])
AC_CONFIG_MACRO_DIR([config/m4])
AC_CONFIG_AUX_DIR([config])
AM_INIT_AUTOMAKE
LT_INIT # Initialize Libtool for shared library support

AC_ARG_ENABLE([debug],
[  --enable-debug    Turn on debugging],
[case "${enableval}" in
  yes) debug=true ;;
  no)  debug=false ;;
  *) AC_MSG_ERROR([bad value ${enableval} for --enable-debug]) ;;
esac],[debug=false])
AM_CONDITIONAL([DEBUG], [test x$debug = xtrue])

LT_INIT

AC_CONFIG_COMMANDS([stash-house-build],
 [echo "(⊃｡•‿•｡)⊃━⭑･ﾟﾟ･*:༅｡.｡༅:*ﾟ:*:✼✿ Configuring for $mypkgname v2.0.0 ☽༓･*˚⁺‧"],
 [mypkgname=$PACKAGE_NAME])

AC_CONFIG_FILES([
  Makefile
])

AC_OUTPUT

