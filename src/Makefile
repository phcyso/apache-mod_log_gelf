builddir=.

ifneq (,$(wildcard /usr/share/apache2/build/special.mk))
  top_srcdir=/usr/share/apache2
  top_builddir=/usr/share/apache2
  include /usr/share/apache2/build/special.mk
endif
ifneq (,$(wildcard /usr/lib64/httpd/build/special.mk))
  top_srcdir=/etc/httpd
  top_builddir=/usr/lib64/httpd
  include /usr/lib64/httpd/build/special.mk
endif
ifneq (,$(wildcard /etc/debian_version))
  DISTRIBUTION := $(shell lsb_release -i | cut -f2)
  ifeq ($(DISTRIBUTION),Debian)
  DEFS=-DWITH_APACHE22
  endif
endif

#   the used tools
APXS=apxs
APACHECTL=apachectl

#   additional defines, includes and libraries
LIBS=-ljson-c -lz

#   the default target
all: local-shared-build

#   install the shared object file into Apache 
install: install-modules-yes

#   cleanup
clean:
	-rm -f mod_log_gelf.o mod_log_gelf.lo mod_log_gelf.slo mod_log_gelf.la 

#   simple test
test: reload
	lynx -mime_header http://localhost

#   install and activate shared object by reloading Apache to
#   force a reload of the shared object file
reload: install restart

#   the general Apache start/restart/stop
#   procedures
start:
	$(APACHECTL) start
restart:
	$(APACHECTL) restart
stop:
	$(APACHECTL) stop

