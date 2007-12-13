## vim:set ft=make ts=4 sw=4 sts=4 sta ai noet:

TARGET :=


SRCS :=
CFGS :=


CC := gcc
CFLAGS := -Wall -O2 -I.
LDFLAGS :=


## =============================================


CSRCS := $(filter %.c, $(SRCS))
CPPSRCS := $(filter %.cpp, $(SRCS))
OBJS := $(CSRCS:.c=.o) $(CPPSRCS:.cpp=.o)
DEPS := $(CSRCS:.c=.d) $(CPPSRCS:.cpp=.d)



all : $(TARGET)



$(TARGET) : $(OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $^

.c.o :
	$(CC) $(CFLAGS) -c -o $@ $<

.cpp.o :
	$(CC) $(CFLAGS) -c -o $@ $<



ifneq ($(MAKECMDGOALS),clean)
ifneq ($(MAKECMDGOALS),mrproper)
ifneq ($(MAKECMDGOALS),distclean)
ifneq ($(MAKECMDGOALS),doc)
ifneq ($(MAKECMDGOALS),unittest)
sinclude $(DEPS)
endif
endif
endif
endif
endif



.PHONY : clean mrproper distclean doc
clean :
	-@rm -f $(OBJS) $(DEPS) $(TARGET)
	-@find . \( -name '*.[oas]' -o -name '*.d' -o -name '*.d.*' \) \
				-type f -print | xargs rm -f

mrproper : clean
	-@rm -f $(CFGS)
	-@find . \( -name tags -o -name cscope.out -o -name cscope.files \) \
				-type f -print | xargs rm -f

distclean : mrproper
	-@find . \( -name '*.orig' -o -name '*.rej' -o -name '*~' \
				-o -name '*.bak' -o -name '#*#' -o -name '*.org' \
				-o -name '.*.rej' -o -size 0 \
				-o -name '*%' -o -name 'core' \) \
				-type f -print | xargs rm -f



%.d : %.c
	@set -e; b=`basename $< .c`; d=`dirname $<`; rm -f $@;  \
	$(CC) -MM $(CFLAGS) $< > $@.$$$$;       \
	sed "s,\($${b}\)\.o[ :]*,$${d}/\1.o $${d}/\1.d : ,g" < $@.$$$$ > $@; \
	[ -s $@ ] && rm -f $@.$$$$

%.d : %.cpp
	@set -e; b=`basename $< .cpp`; d=`dirname $<`; rm -f $@;  \
	$(CC) -MM $(CFLAGS) $< > $@.$$$$;       \
	sed "s,\($${b}\)\.o[ :]*,$${d}/\1.o $${d}/\1.d : ,g" < $@.$$$$ > $@; \
	[ -s $@ ] && rm -f $@.$$$$



doc :
	@-rm -fr html
	@doxygen DOCUMENT/Doxyfile



## unit test를 수행한다.
unittest :
	@TEST_DIR=`find . -type d -name "unittest" -print`; \
	for tdir in $${TEST_DIR}; do make -C $${tdir} || exit 1; done;  \
	for tdir in $${TEST_DIR}; do make -C $${tdir} run; done
