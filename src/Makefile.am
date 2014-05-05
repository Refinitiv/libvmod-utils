INCLUDES = -I$(VARNISHSRC)/include -I$(VARNISHSRC) -I$(VARNISHSRC)/bin/varnishd/

if VARNISH3
INCLUDES+=-I$(VARNISHSRC)/bin/varnishd/cache
endif

vmoddir = $(VMODDIR)
vmod_LTLIBRARIES = libvmod_utils.la

libvmod_utils_la_LDFLAGS = -module -export-dynamic -avoid-version

libvmod_utils_la_SOURCES = \
	vcc_if.c \
	vcc_if.h \
	vmod_utils.c

vcc_if.c vcc_if.h: $(VARNISHSRC)/lib/libvmod_std/vmod.py $(top_srcdir)/src/vmod_utils.vcc
	@PYTHON@ $(VARNISHSRC)/lib/libvmod_std/vmod.py $(top_srcdir)/src/vmod_utils.vcc

VMOD_TESTS = tests/*.vtc
.PHONY: $(VMOD_TESTS)

tests/*.vtc:
	$(VARNISHSRC)/bin/varnishtest/varnishtest -l -Dvarnishd=$(VARNISHSRC)/bin/varnishd/varnishd -Dvmod_topbuild=$(abs_top_builddir) $@

check: $(VMOD_TESTS)

EXTRA_DIST = \
	vmod_utils.vcc \
	$(VMOD_TESTS)

CLEANFILES = $(builddir)/vcc_if.c $(builddir)/vcc_if.h
