tooldir = $(top_builddir)/src/tools
if ENABLE_BINARY_DATA
chindexs = ch_index_begin.dat \
		   ch_index_phone.dat
else
chindexs = ch_index.dat
endif
datas = \
	us_freq.dat \
	dict.dat \
	ph_index.dat \
	fonetree.dat \
	$(chindexs)
static_tables = pinyin.tab swkb.dat symbols.dat
generated_header = $(top_builddir)/src/chewing-definition.h

all: checkdata_stamp gendata_stamp

checkdata_stamp: phone.cin tsi.src
	@echo "timestamp" > $@

gendata_stamp:
	if test -f $(generated_header); then \
		echo "chewing-definition.h exists."; \
	else \
		$(MAKE) gendata; \
	fi
	@echo "timestamp" > $@

gendata:
	$(tooldir)/sort_word $(top_srcdir)/data/phone.cin
	$(tooldir)/sort_dic $(top_srcdir)/data/tsi.src
	$(tooldir)/maketree
	-rm -f phoneid.dic
	-mv -f chewing-definition.h $(top_builddir)/src/

install-static-tables: $(static_tables)
	$(foreach file,$(static_tables), \
	  $(INSTALL_DATA) $(top_srcdir)/data/$(file) $(DESTDIR)/$(datadir)/chewing;)

install-data-local: $(datas) install-static-tables
	$(mkinstalldirs) $(DESTDIR)/$(datadir)/chewing
	$(INSTALL_DATA) $(datas) $(DESTDIR)/$(datadir)/chewing

CLEANFILES = $(datas) gendata_stamp checkdata_stamp $(generated_header)
