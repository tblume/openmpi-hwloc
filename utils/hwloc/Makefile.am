# Copyright © 2009-2015 Inria.  All rights reserved.
# Copyright © 2009-2012, 2014 Université Bordeaux
# Copyright © 2009-2014 Cisco Systems, Inc.  All rights reserved.
#
# See COPYING in top-level directory.

# This makefile is only reached when building in standalone mode

AM_CFLAGS = $(HWLOC_CFLAGS)
AM_CPPFLAGS = $(HWLOC_CPPFLAGS)
AM_LDFLAGS = $(HWLOC_LDFLAGS)

LDADD = $(HWLOC_top_builddir)/hwloc/libhwloc.la

EXTRA_DIST = \
        test-hwloc-annotate.input test-hwloc-annotate.output \
        test-hwloc-calc.output \
        test-hwloc-compress-dir.input.tar.gz test-hwloc-compress-dir.output.tar.gz \
        test-hwloc-diffpatch.input1 test-hwloc-diffpatch.input2 \
        test-hwloc-distances.output \
        test-hwloc-distrib.output \
        test-hwloc-info.output

noinst_HEADERS = misc.h

bin_PROGRAMS = \
        hwloc-annotate \
        hwloc-bind \
        hwloc-calc \
        hwloc-diff \
        hwloc-distances \
        hwloc-distrib \
        hwloc-info \
        hwloc-patch \
        hwloc-ps

if HWLOC_HAVE_X86_CPUID
bin_PROGRAMS += hwloc-gather-cpuid
endif HWLOC_HAVE_X86_CPUID

if HWLOC_HAVE_LINUX
if HWLOC_HAVE_X86
sbin_PROGRAMS = hwloc-dump-hwdata
endif HWLOC_HAVE_X86
endif HWLOC_HAVE_LINUX

hwloc_dump_hwdata_SOURCES = \
	hwloc-dump-hwdata.c \
	hwloc-dump-hwdata-knl.c

hwloc_calc_SOURCES = \
        hwloc-calc.h \
        hwloc-calc.c

bin_SCRIPTS = hwloc-compress-dir
if HWLOC_HAVE_LINUX
bin_SCRIPTS += hwloc-gather-topology
endif HWLOC_HAVE_LINUX

if !HWLOC_HAVE_MINGW32
TESTS = \
        test-hwloc-annotate.sh \
        test-hwloc-calc.sh \
        test-hwloc-compress-dir.sh \
        test-hwloc-diffpatch.sh \
        test-hwloc-distances.sh \
        test-hwloc-distrib.sh \
        test-hwloc-info.sh
if HWLOC_HAVE_PLUGINS
TESTS += test-fake-plugin.sh
endif HWLOC_HAVE_PLUGINS
endif !HWLOC_HAVE_MINGW32

# Only install man pages if we're building in standalone mode
man7_pages = hwloc.7
EXTRA_DIST += $(man7_pages:.7=.7in)
nodist_man_MANS = $(man7_pages)

man1_pages = \
        hwloc-annotate.1 \
        hwloc-bind.1 \
        hwloc-calc.1 \
        hwloc-compress-dir.1 \
        hwloc-diff.1 \
        hwloc-distances.1 \
        hwloc-distrib.1 \
        hwloc-info.1 \
        hwloc-patch.1 \
        hwloc-ps.1
if HWLOC_HAVE_X86_CPUID
man1_pages += hwloc-gather-cpuid.1
endif HWLOC_HAVE_X86_CPUID

if HWLOC_HAVE_LINUX
if HWLOC_HAVE_X86
man1_pages += hwloc-dump-hwdata.1in
endif HWLOC_HAVE_X86
endif HWLOC_HAVE_LINUX

EXTRA_DIST += $(man1_pages:.1=.1in)
nodist_man_MANS += $(man1_pages)

# Only install the gather-topology page if we're on Linux, but we need
# to include it in the tarball, regardless of what OS we're building
# on.
hgt_page = hwloc-gather-topology.1
EXTRA_DIST += $(hgt_page:.1=.1in)
if HWLOC_HAVE_LINUX
nodist_man_MANS += $(hgt_page)
endif HWLOC_HAVE_LINUX

# Same for dump-hwdata page on Linux/x86
hdh_page = hwloc-dump-hwdata.1
EXTRA_DIST += $(hdh_page:.1=.1in)
if HWLOC_HAVE_LINUX
if HWLOC_HAVE_X86
nodist_man_MANS += $(hdh_page)
endif HWLOC_HAVE_X86
endif HWLOC_HAVE_LINUX


.1in.1:
	@ echo Creating $@ man page...
	@ $(SED) -e 's/#PACKAGE_NAME#/@PACKAGE_NAME@/g' \
	  -e 's/#PACKAGE_VERSION#/@PACKAGE_VERSION@/g' \
	  -e 's/#HWLOC_DATE#/@HWLOC_RELEASE_DATE@/g' \
	  > $@ < $<

.3in.3:
	@ echo Creating $@ man page...
	@ $(SED) -e 's/#PACKAGE_NAME#/@PACKAGE_NAME@/g' \
	  -e 's/#PACKAGE_VERSION#/@PACKAGE_VERSION@/g' \
	  -e 's/#HWLOC_DATE#/@HWLOC_RELEASE_DATE@/g' \
	  > $@ < $<

.7in.7:
	@ echo Creating $@ man page...
	@ $(SED) -e 's/#PACKAGE_NAME#/@PACKAGE_NAME@/g' \
	  -e 's/#PACKAGE_VERSION#/@PACKAGE_VERSION@/g' \
	  -e 's/#HWLOC_DATE#/@HWLOC_RELEASE_DATE@/g' \
	  > $@ < $<

install-exec-hook:
	$(SED) -e 's/HWLOC_top_builddir\/utils\/hwloc/bindir/' -e 's/HWLOC_top_builddir\/utils\/lstopo/bindir/' -e '/HWLOC_top_builddir/d' $(DESTDIR)$(bindir)/hwloc-compress-dir > $(DESTDIR)$(bindir)/hwloc-compress-dir.tmp && mv -f $(DESTDIR)$(bindir)/hwloc-compress-dir.tmp $(DESTDIR)$(bindir)/hwloc-compress-dir
	chmod +x $(DESTDIR)$(bindir)/hwloc-compress-dir
if HWLOC_HAVE_LINUX
	$(SED) -e 's/HWLOC_top_builddir\/utils\/lstopo/bindir/' -e '/HWLOC_top_builddir/d' $(DESTDIR)$(bindir)/hwloc-gather-topology > $(DESTDIR)$(bindir)/hwloc-gather-topology.tmp && mv -f $(DESTDIR)$(bindir)/hwloc-gather-topology.tmp $(DESTDIR)$(bindir)/hwloc-gather-topology
	chmod +x $(DESTDIR)$(bindir)/hwloc-gather-topology
endif HWLOC_HAVE_LINUX

distclean-local:
	rm -f $(nodist_man_MANS)
