# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.68])

m4_define([PKG_NAME], firewalld)
m4_define([PKG_VERSION], m4_bpatsubst(m4_esyscmd([grep "Version:" firewalld.spec]), [Version:\W\([0-9.]*\)\W], [\1]))
m4_define([PKG_RELEASE], m4_bpatsubst(m4_esyscmd([grep "Release:" firewalld.spec]), [Release:\W\([0-9.]*\).*\W], [\1]))
m4_define([PKG_TAG], m4_format(v%s, PKG_VERSION))

AC_INIT(PKG_NAME,PKG_VERSION)
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR([src/firewalld.in])
AC_PREFIX_DEFAULT([/usr])

AM_INIT_AUTOMAKE([1.11 tar-ustar no-define foreign dist-bzip2 no-dist-gzip])

AC_SUBST([PACKAGE_RELEASE], '[PKG_RELEASE]')
AC_DEFINE_UNQUOTED([PACKAGE_RELEASE], ["$PACKAGE_RELEASE"])
AC_SUBST([PACKAGE_TAG], '[PKG_TAG]')
AC_DEFINE_UNQUOTED([PACKAGE_TAG], ["$PACKAGE_TAG"])

AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MKDIR_P
AC_PROG_SED
AC_PROG_GREP
AC_PROG_AWK
AC_PROG_MAKE_SET
AM_PATH_PYTHON([3.6])

AC_PATH_PROG([XSLTPROC], [xsltproc])
AC_PATH_PROG([KILL], [kill], [/usr/bin/kill])

AC_PATH_PROG([MODPROBE], [modprobe], [/sbin/modprobe])
AC_PATH_PROG([RMMOD], [rmmod], [/sbin/rmmod])
AC_PATH_PROG([SYSCTL], [sysctl], [/sbin/sysctl])

AC_CONFIG_TESTDIR([src/tests])
AC_PATH_PROGS([PODMAN], [podman docker], [/bin/false])

GLIB_GSETTINGS

#############################################################

AC_ARG_ENABLE([docs],
    [AS_HELP_STRING([--disable-docs], [Disable building documentation])])
AM_CONDITIONAL([ENABLE_DOCS], [test x$enable_docs != xno])
AM_COND_IF([ENABLE_DOCS], [
    JH_CHECK_XML_CATALOG([http://docbook.sourceforge.net/release/xsl/current/manpages/docbook.xsl], [DocBook XSL Stylesheets])
])

#############################################################

AC_ARG_ENABLE([systemd],
       AS_HELP_STRING([--disable-systemd], [Disable systemd support]),
       [use_systemd=$enableval], [use_systemd=yes])
AM_CONDITIONAL(USE_SYSTEMD, test x$use_systemd = xyes)
AC_SUBST(USE_SYSTEMD)

AC_ARG_ENABLE([sysconfig],
    [AS_HELP_STRING([--enable-sysconfig], [Install sysconfig file])],
    [INSTALL_SYSCONFIG="${enableval}"], [INSTALL_SYSCONFIG='no'])
AM_CONDITIONAL(INSTALL_SYSCONFIG, [test x$INSTALL_SYSCONFIG = xyes])
AC_SUBST(INSTALL_SYSCONFIG)

AC_ARG_ENABLE([rpmmacros],
    [AS_HELP_STRING([--enable-rpmmacros], [Install rpm macros file])],
    [INSTALL_RPMMACROS="${enableval}"], [INSTALL_RPMMACROS='no'])
AM_CONDITIONAL(INSTALL_RPMMACROS, [test x$INSTALL_RPMMACROS = xyes])
AC_SUBST(INSTALL_RPMMACROS)

AC_ARG_WITH([systemd-unitdir],
       AS_HELP_STRING([--with-systemd-unitdir], [Directory for systemd service files]),
       [SYSTEMD_UNITDIR=$withval], [SYSTEMD_UNITDIR="\${prefix}/lib/systemd/system"])
AC_SUBST(SYSTEMD_UNITDIR)

AC_ARG_WITH([bashcompletiondir],
       AS_HELP_STRING([--with-bashcompletiondir=DIR], [Bash completions directory]),
       [BASHCOMPLETIONDIR=$withval], [BASHCOMPLETIONDIR="${datadir}/bash-completion/completions"])
AC_SUBST(BASHCOMPLETIONDIR)

AC_ARG_WITH([zshcompletiondir],
       AS_HELP_STRING([--with-zshcompletiondir=DIR], [Zsh completions directory]),
       [ZSHCOMPLETIONDIR=$withval], [ZSHCOMPLETIONDIR="${datadir}/zsh/site-functions"])
AC_SUBST(ZSHCOMPLETIONDIR)

AC_ARG_WITH([ifcfgdir],
       AS_HELP_STRING([--with-ifcfgdir=DIR], [The ifcfg configuration directory]),
       [IFCFGDIR=$withval], [IFCFGDIR="/etc/sysconfig/network-scripts"])
AC_SUBST(IFCFGDIR)

# Extend PATH to include /sbin etc in case we are building as non-root
FW_TOOLS_PATH="$PATH:/usr/local/sbin:/sbin:/usr/sbin"

AC_ARG_WITH([iptables],
       AS_HELP_STRING([--with-iptables], [Path to iptables executable]),
       [IPTABLES=$withval
       AC_MSG_NOTICE([Using for iptables: $IPTABLES])],
       [AC_PATH_PROG([IPTABLES], [iptables], [/bin/false], [$FW_TOOLS_PATH])])
AC_SUBST(IPTABLES)

AC_ARG_WITH([iptables-restore],
       AS_HELP_STRING([--with-iptables-restore], [Path to iptables-restore executable]),
       [IPTABLES_RESTORE=$withval
       AC_MSG_NOTICE([Using for iptables-restore: $IPTABLES_RESTORE])],
       [AC_PATH_PROG([IPTABLES_RESTORE], [iptables-restore], [/bin/false], [$FW_TOOLS_PATH])])
AC_SUBST(IPTABLES_RESTORE)

AC_ARG_WITH([ip6tables],
       AS_HELP_STRING([--with-ip6tables], [Path to ip6tables executable]),
       [IP6TABLES=$withval
       AC_MSG_NOTICE([Using for ip6tables: $IP6TABLES])],
       [AC_PATH_PROG([IP6TABLES], [ip6tables], [/bin/false], [$FW_TOOLS_PATH])])
AC_SUBST(IP6TABLES)

AC_ARG_WITH([ip6tables-restore],
       AS_HELP_STRING([--with-ip6tables-restore], [Path to ip6tables-restore executable]),
       [IP6TABLES_RESTORE=$withval
       AC_MSG_NOTICE([Using for ip6tables-restore: $IP6TABLES_RESTORE])],
       [AC_PATH_PROG([IP6TABLES_RESTORE], [ip6tables-restore], [/bin/false], [$FW_TOOLS_PATH])])
AC_SUBST(IP6TABLES_RESTORE)

AC_ARG_WITH([ebtables],
       AS_HELP_STRING([--with-ebtables], [Path to ebtables executable]),
       [EBTABLES=$withval
       AC_MSG_NOTICE([Using for ebtables: $EBTABLES])],
       [AC_PATH_PROG([EBTABLES], [ebtables], [/bin/false], [$FW_TOOLS_PATH])])
AC_SUBST(EBTABLES)

AC_ARG_WITH([ebtables-restore],
       AS_HELP_STRING([--with-ebtables-restore], [Path to ebtables-restore executable]),
       [EBTABLES_RESTORE=$withval
       AC_MSG_NOTICE([Using for ebtables-restore: $EBTABLES_RESTORE])],
       [AC_PATH_PROG([EBTABLES_RESTORE], [ebtables-restore], [/bin/false], [$FW_TOOLS_PATH])])
AC_SUBST(EBTABLES_RESTORE)

AC_ARG_WITH([ipset],
       AS_HELP_STRING([--with-ipset], [Path to ipset executable]),
       [IPSET=$withval
       AC_MSG_NOTICE([Using for ipset: $IPSET])],
       [AC_PATH_PROG([IPSET], [ipset], [/bin/false], [$FW_TOOLS_PATH])])
AC_SUBST(IPSET)

DEFAULT_LOG_TARGET=${DEFAULT_LOG_TARGET:-mixed}
AC_ARG_VAR([DEFAULT_LOG_TARGET], m4_flatten([
            Select the default logging backend. One of: mixed, syslog, file, console.
            Mixed means both syslog and file targets. Mixed is the default.
           ]))

#############################################################

AC_SUBST([GETTEXT_PACKAGE], '[PKG_NAME]')
AC_DEFINE_UNQUOTED([GETTEXT_PACKAGE], ["$GETTEXT_PACKAGE"],)

IT_PROG_INTLTOOL([0.35.0], [no-xml])
AM_PO_SUBDIRS

AC_CONFIG_COMMANDS([xsl-cleanup],,[rm -f doc/xml/transform-*.xsl])

AC_CONFIG_FILES([Makefile
		 doxygen.conf
		 config/Makefile
		 doc/Makefile
		 doc/man/Makefile
		 doc/man/man1/Makefile
		 doc/man/man5/Makefile
		 doc/xml/Makefile
		 po/Makefile.in
		 shell-completion/Makefile
		 src/firewall/config/__init__.py
		 src/Makefile
		 src/tests/Makefile
		 src/tests/atlocal
		 src/icons/Makefile])

m4_foreach([FILE], [[src/firewall-applet],
                   [src/firewall-cmd],
                   [src/firewall-offline-cmd],
                   [src/firewall-config],
                   [src/firewalld]],
    [AC_CONFIG_FILES(FILE, chmod +x FILE)]
)

AC_OUTPUT
