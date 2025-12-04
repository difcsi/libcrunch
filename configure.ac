AC_CONFIG_MACRO_DIR([m4])
AC_PREREQ([2.69])
AC_INIT([libcrunch], [0.1], [srk31@cl.cam.ac.uk])
AC_CONFIG_SRCDIR([src/libcrunch.c])
AC_CONFIG_HEADERS([config.h])
AC_PREFIX_DEFAULT(/usr/local)

AS_IF([test "x$prefix" = xNONE], [prefix=/usr/local])

CFLAGS="-I$prefix/include $CFLAGS"
CXXFLAGS="-I$prefix/include $CXXFLAGS"
CPPFLAGS="-I$prefix/include $CPPFLAGS"
LDFLAGS="-L$prefix/lib $LDFLAGS"
PKG_CONFIG_PATH="$prefix/lib/pkgconfig:$PKG_CONFIG_PATH"
export CFLAGS CXXFLAGS LDFLAGS PKG_CONFIG_PATH

AC_LANG([C])
AM_INIT_AUTOMAKE([foreign subdir-objects -Wno-portability])
AM_MAINTAINER_MODE
AC_PROG_CC
AC_PROG_CC_C99

AC_C_INLINE
AC_TYPE_SIZE_T
AC_CHECK_TYPES([ptrdiff_t])
AC_PROG_LN_S
AC_PROG_INSTALL

my_save_cflags="$CFLAGS"
CFLAGS="-flto -ffat-lto-objects"
AC_MSG_CHECKING([whether $CC supports -flto -ffat-lto-objects])
AC_COMPILE_IFELSE([AC_LANG_PROGRAM([])],
    [AC_MSG_RESULT([yes])]
    [],
    [AC_MSG_FAILURE([need a C compiler that supports -flto (e.g. gcc >= 4.8) Try again with CC=path/to/newer-cc?])]
)
CFLAGS="$my_save_cflags"

AC_CHECK_HEADERS([assert.h dlfcn.h elf.h], [], [AC_MSG_FAILURE(Failed to find required header)])
AC_CHECK_HEADERS([err.h errno.h libunwind.h link.h pthread.h search.h], [], [AC_MSG_FAILURE(Failed to find required header)])
AC_CHECK_HEADERS([stdarg.h stdbool.h stddef.h stdint.h stdio.h stdlib.h string.h strings.h], [], [AC_MSG_FAILURE(Failed to find required header)])
AC_CHECK_HEADERS([sys/mman.h sys/resource.h sys/stat.h sys/syscall.h sys/time.h sys/types.h unistd.h], [], [AC_MSG_FAILURE(Failed to find required header)])
AC_CHECK_HEADERS([alloca.h], [], [AC_MSG_FAILURE(Failed to find required header)])

AC_ARG_ENABLE([debug], AS_HELP_STRING([--enable-debug], [Enable debug mode]),
              [enable_debug=${enableval}])
AC_ARG_ENABLE([fake-libunwind],
              AS_HELP_STRING([--enable-fake-libunwind], [Use our own fake libunwind (faster but less stable)]),
              [enable_fake_libunwind=${enableval}])

AS_IF([test "x$enable_fake_libunwind" = "xyes"],
      [AC_DEFINE([USE_FAKE_LIBUNWIND],1,[Defined if using our own version of libunwind])])

AC_ARG_WITH([liballocs],
            [AS_HELP_STRING([--with-liballocs=DIR],
              [where to find liballocs (headers and libraries)])],
            [
                if test -d "$withval"
                then
                        ac_liballocs="$withval"
                else
                        AC_MSG_ERROR(--with-liballocs expected directory name)
                fi
            ],
            [ac_liballocs=${LIBALLOCS:-$(realpath $srcdir)/contrib/liballocs}])
AC_ARG_WITH([libsystrap],
            [AS_HELP_STRING([--with-libsystrap=DIR],
              [where to find libsystrap (headers and source)])],
            [
                if test -d "$withval"
                then
                        ac_libsystrap="$withval"
                else
                        AC_MSG_ERROR(--with-libsystrap expected directory name)
                fi
            ],
            [ac_libsystrap=${LIBSYSTRAP:-$(realpath $srcdir)/contrib/liballocs/contrib/libsystrap]})
AC_ARG_WITH([librunt],
            [AS_HELP_STRING([--with-librunt=DIR],
              [where to find librunt (headers and source)])],
            [
                if test -d "$withval"
                then
                        ac_librunt="$withval"
                else
                        AC_MSG_ERROR(--with-librunt expected directory name)
                fi
            ],
            [ac_librunt=${LIBRUNT:-$(realpath $srcdir)/contrib/liballocs/contrib/libsystrap/contrib/librunt]})
AC_ARG_WITH([libdlbind],
            [AS_HELP_STRING([--with-libdlbind=DIR],
              [where to find libdlbind (headers, source)])],
            [
                if test -d "$withval"
                then
                        ac_libdlbind="$withval"
                else
                        AC_MSG_ERROR(--with-libdlbind expected directory name)
                fi
            ],
            [ac_libdlbind=${LIBDLBIND:-$(realpath $srcdir)/contrib/liballocs/contrib/libdlbind]})
AC_ARG_WITH([toolsub],
            [AS_HELP_STRING([--with-toolsub=DIR],
              [where to find toolsub (pre-built source tree)])],
            [
                if test -d "$withval"
                then
                        ac_toolsub="$withval"
                else
                        AC_MSG_ERROR(--with-toolsub expected directory name)
                fi
            ],
            [ac_toolsub=${TOOLSUB:-$(realpath $srcdir)/contrib/liballocs/contrib/toolsub]})
AC_ARG_WITH([elftin],
            [AS_HELP_STRING([--with-elftin=DIR],
              [where to find elftin (pre-built source tree)])],
            [
                if test -d "$withval"
                then
                        ac_elftin="$withval"
                else
                        AC_MSG_ERROR(--with-elftin expected directory name)
                fi
            ],
            [ac_elftin=${ELFTIN:-$(realpath $srcdir)/contrib/liballocs/contrib/elftin]})
AC_ARG_WITH([cil-install],
            [AS_HELP_STRING([--with-cil-install=DIR],
              [where to find CIL libraries (cil/cil.cmxa)])],
            [
                if test -d "$withval"
                then
                        ac_cil_install="$withval"
                else
                        AC_MSG_ERROR(--with-cil-install expected directory name)
                fi
            ],
            [ac_cil_install=${CIL_INSTALL:-$(realpath $srcdir)/contrib/liballocs/contrib/cil/lib]})


# things on the left will be substituted, if @-surrounded, by the right, in .in files
AC_SUBST([liballocs], [$ac_liballocs])
AC_SUBST([librunt], [$ac_librunt])
AC_SUBST([libsystrap], [$ac_libsystrap])
AC_SUBST([libdlbind], [$ac_libdlbind])
AC_SUBST([toolsub], [$ac_toolsub])
AC_SUBST([elftin], [$ac_elftin])
AC_SUBST([cil_install], [$ac_cil_install])

AM_CONDITIONAL(DEBUG, [test "x$enable_debug" = "xyes"])
AM_CONDITIONAL(USE_FAKE_LIBUNWIND, [test "x$enable_fake_libunwind" = "xyes"])

AC_CONFIG_FILES([Makefile config.mk])
AC_OUTPUT
