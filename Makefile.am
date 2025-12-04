ACLOCAL_AMFLAGS = -I m4

SUBDIRS = 

# Main targets
all-local:
	$(MAKE) -f Makefile src lib frontend

src:
	$(MAKE) -f Makefile -C src

lib: src
	$(MAKE) -f Makefile -C lib

frontend: lib
	$(MAKE) -f Makefile -C frontend

test: frontend
	$(MAKE) -f Makefile -C test

clean-local:
	$(MAKE) -C src clean
	$(MAKE) -C lib clean
	$(MAKE) -C frontend clean

.PHONY: src lib frontend test
