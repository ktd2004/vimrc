## vim:set ft=make ts=4 sw=4 sts=4 sta ai noet:

TARGET :=


SRCS :=
OBJS := $(SRCS:.c=.o)
DEPS := $(SRCS:.c=.d)


CC := gcc
CFLAGS := -Wall -O2 -I.
LDFLAGS :=



all : $(TARGET)



$(TARGET) : $(OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $^

.c.o :
	$(CC) $(CFLAGS) -c -o $@ $<



ifneq ($(MAKECMDGOALS),clean)
ifneq ($(MAKECMDGOALS),mrproper)
ifneq ($(MAKECMDGOALS),doc)
ifneq ($(MAKECMDGOALS),unittest)
sinclude $(DEPS)
endif
endif
endif
endif



.PHONY : clean mrproper doc
clean :
	rm -f $(OBJS)

mrproper : clean
	rm -f $(TARGET) $(DEPS)



%.d : %.c
	@set -e; b=`basename $< .c`; d=`dirname $<`; rm -f $@;  \
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
