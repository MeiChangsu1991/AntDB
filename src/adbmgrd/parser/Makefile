#-------------------------------------------------------------------------
#
# Makefile for parser
#
# src/adbmgrd/parser/Makefile
#
#-------------------------------------------------------------------------

subdir = src/adbmgrd/parser
top_builddir = ../../..
include $(top_builddir)/src/Makefile.global

override CPPFLAGS := -I. -I$(srcdir) $(CPPFLAGS)

OBJS= analyze.o gram.o parser.o keywords.o \
      parse_agg.o parse_clause.o parse_coerce.o parse_collate.o parse_cte.o \
      parse_expr.o parse_func.o parse_node.o parse_oper.o parse_param.o \
      parse_relation.o parse_target.o parse_type.o parse_utilcmd.o scansup.o \
      mgr_gram.o mgr_kwlist.o scan.o

include $(top_srcdir)/src/adbmgrd/common.mk


# scan is compiled as part of gram
gram.o mgr_gram.o: scan.c

# Latest flex causes warnings in this file.
ifeq ($(GCC),yes)
gram.o mgr_gram.o: CFLAGS += -Wno-error
scan.o: CFLAGS += -Wno-error
endif

# There is no correct way to write a rule that generates two files.
# Rules with two targets don't have that meaning, they are merely
# shorthand for two otherwise separate rules.  To be safe for parallel
# make, we must chain the dependencies like this.  The semicolon is
# important, otherwise make will choose the built-in rule for
# gram.y=>gram.c.

gram.h: gram.c ;
mgr_gram.h: mgr_gram.c ;

gram.c mgr_gram.c: BISONFLAGS += -d
#gram.c: BISON_CHECK_CMD = $(PERL) $(backend_src_dir)/check_keywords.pl $< $(mgr_inc_dir)/parser/kwlist.h


scan.c: FLEXFLAGS = -CF -p -p
scan.c: FLEX_NO_BACKUP=yes


# Force these dependencies to be known even without dependency info built:
gram.o keywords.o parser.o: gram.h
mgr_gram.o mgr_kwlist.o: mgr_gram.h

clean:
	rm -f gram.c gram.h scan.c mgr_gram.c mgr_gram.h lex.backup

distclean maintainer-clean:
	rm -f lex.backup

keywords.c:$(top_srcdir)/src/common/keywords.c
	$(LN_S) $<
