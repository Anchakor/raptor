# -*- Mode: Makefile -*-
#
# Makefile.am - automake file for Raptor N-Triples tests
#
# Copyright (C) 2000-2009, David Beckett http://www.dajobe.org/
# Copyright (C) 2000-2004, University of Bristol, UK http://www.bristol.ac.uk/
# 
# This package is Free Software and part of Redland http://librdf.org/
# 
# It is licensed under the following three licenses as alternatives:
#   1. GNU Lesser General Public License (LGPL) V2.1 or any newer version
#   2. GNU General Public License (GPL) V2 or any newer version
#   3. Apache License, V2.0 or any newer version
# 
# You may not use this file except in compliance with at least one of
# the above three licenses.
# 
# See LICENSE.html or LICENSE.txt at the top of this package for the
# complete terms and further detail along with the license texts for
# the licenses in COPYING.LIB, COPYING and LICENSE-2.0.txt respectively.
# 
# 

NT_TEST_FILES=test.nt

NT_OUT_FILES=test.out

NT_BAD_TEST_FILES=bad-00.nt bad-01.nt bad-02.nt bad-03.nt bad-04.nt \
bad-05.nt bad-06.nt bad-07.nt

NQ_TEST_FILES=testnq-1.nq testnq-optional-context.nq bug-481.nq

NQ_OUT_FILES=testnq-1.out testnq-optional-context.out bug-481.out

# Used to make N-triples output consistent
BASE_URI=http://librdf.org/raptor/tests/

EXTRA_DIST = \
	$(NT_TEST_FILES) \
	$(NT_OUT_FILES) \
	$(NT_BAD_TEST_FILES) \
	$(NQ_TEST_FILES) \
	$(NQ_OUT_FILES)


build-rapper:
	@(cd $(top_builddir)/utils ; $(MAKE) rapper$(EXEEXT))

check-local: build-rapper \
check-nt check-bad-nt check-nq

check-nt: build-rapper $(NT_TEST_FILES)
	@set +e; result=0; \
	$(RECHO) "Testing N-Triples"; \
	for test in $(NT_TEST_FILES); do \
	  name=`basename $$test .nt` ; \
	  $(RECHO) $(RECHO_N) "Checking $$test $(RECHO_C)"; \
	  $(top_builddir)/utils/rapper -q -i ntriples -o ntriples file:$(srcdir)/$$test $(BASE_URI)$$test > $$name.res 2>/dev/null; \
	  if cmp $(srcdir)/$$name.out $$name.res >/dev/null 2>&1; then \
	    $(RECHO) "ok"; \
	  else \
	    $(RECHO) "FAILED"; \
	    diff $(srcdir)/$$name.out $$name.res; result=1; \
	  fi; \
	  rm -f $$name.res ; \
	done; \
	set -e; exit $$result

check-bad-nt: build-rapper $(NT_BAD_TEST_FILES)
	@set +e; result=0; \
	$(RECHO) "Testing that bad N-Triples fails"; \
	for test in $(NT_BAD_TEST_FILES); do \
	  name=`basename $$test .nt` ; \
	  baseuri=$(BASE_URI)$$name.nt; \
	  $(RECHO) $(RECHO_N) "Checking $$test $(RECHO_C)"; \
	  $(top_builddir)/utils/rapper -q -i ntriples -o ntriples file:$(srcdir)/$$test $$baseuri > $$name.res 2> $$name.err; \
	  status=$$?; \
	  if test $$status -eq 1 ; then \
	    $(RECHO) "ok"; \
	  elif test $$status -eq 2 ; then \
	    $(RECHO) "FAILED - parsing succeeded with a warning"; \
	    cat $$name.res; grep Warning $$name.err; result=1; \
	  elif test $$status -eq 0 ; then \
	    $(RECHO) "FAILED - parsing succeeded but should have failed"; \
	    cat $$name.res; result=1; \
	  else \
	    $(RECHO) "FAILED - parsing failed with unknown status $$status"; \
	    cat $$name.res; result=1; \
	  fi; \
	  rm -f $$name.res $$name.err ; \
	done; \
	set -e; exit $$result

check-nq: build-rapper $(NQ_TEST_FILES)
	@set +e; result=0; \
	$(RECHO) "Testing N-Quads"; \
	for test in $(NQ_TEST_FILES); do \
	  name=`basename $$test .nq` ; \
	  $(RECHO) $(RECHO_N) "Checking $$test $(RECHO_C)"; \
	  $(top_builddir)/utils/rapper -q -i nquads -o nquads file:$(srcdir)/$$test $(BASE_URI)$$test > $$name.res 2>/dev/null; \
	  if cmp $(srcdir)/$$name.out $$name.res >/dev/null 2>&1; then \
	    $(RECHO) "ok"; \
	  else \
	    $(RECHO) "FAILED"; \
	    diff $(srcdir)/$$name.out $$name.res; result=1; \
	  fi; \
	  rm -f $$name.res ; \
	done; \
	set -e; exit $$result

print-nt-test-files:
	@echo $(NT_TEST_FILES) | tr ' ' '\012'
