AM_YFLAGS = -d

COMMON_CFLAGS = -Wall
COMMON_CPPFLAGS = \
	-include $(top_builddir)/config.h \
	-DBSD=1 -DSHELL \
	-DIFS_BROKEN

AM_CFLAGS = $(COMMON_CFLAGS)
AM_CPPFLAGS = $(COMMON_CPPFLAGS)
AM_CFLAGS_FOR_BUILD = -g -O2 $(COMMON_CFLAGS) 
AM_CPPFLAGS_FOR_BUILD = $(COMMON_CPPFLAGS)

COMPILE_FOR_BUILD = \
	$(CC_FOR_BUILD) $(DEFAULT_INCLUDES) $(AM_CPPFLAGS_FOR_BUILD) \
	$(CPPFLAGS_FOR_BUILD) \
	$(AM_CFLAGS_FOR_BUILD) $(CFLAGS_FOR_BUILD) 

bin_PROGRAMS = mjsh

mjsh_CFILES = \
	alias.c arith_yacc.c arith_yylex.c cd.c error.c eval.c exec.c expand.c \
	histedit.c input.c jobs.c mail.c main.c memalloc.c miscbltin.c \
	mystring.c options.c parser.c redir.c show.c trap.c output.c \
	bltin/printf.c system.c bltin/test.c bltin/times.c var.c
mjsh_SOURCES = \
	$(mjsh_CFILES) \
	alias.h arith_yacc.h bltin/bltin.h cd.h error.h eval.h exec.h \
	expand.h hetio.h \
	init.h input.h jobs.h machdep.h mail.h main.h memalloc.h miscbltin.h \
	myhistedit.h mystring.h options.h output.h parser.h redir.h shell.h \
	show.h system.h trap.h var.h
mjsh_LDADD = builtins.o init.o nodes.o signames.o syntax.o

HELPERS = mkinit mksyntax mknodes mksignames

BUILT_SOURCES = builtins.h nodes.h syntax.h token.h token_vars.h
CLEANFILES = \
	$(BUILT_SOURCES) $(patsubst %.o,%.c,$(mjsh_LDADD)) \
	$(HELPERS) builtins.def

man_MANS = mjsh.1
EXTRA_DIST = \
	$(man_MANS) \
	mktokens mkbuiltins builtins.def.in mkinit.c \
	mknodes.c nodetypes nodes.c.pat mksyntax.c mksignames.c

token.h token_vars.h: mktokens
	sh $^

builtins.def: builtins.def.in $(top_builddir)/config.h
	$(COMPILE) -E -x c -o $@ $<

builtins.c builtins.h: mkbuiltins builtins.def
	sh $^

init.c: mkinit $(mjsh_CFILES)
	./$^

nodes.c nodes.h: mknodes nodetypes nodes.c.pat
	./$^

syntax.c syntax.h: mksyntax
	./$^

signames.c: mksignames
	./$^

mksyntax: token.h

$(HELPERS): %: %.c
	$(COMPILE_FOR_BUILD) -o $@ $<
