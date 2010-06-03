## vim:set ft=make ts=4 sw=4 sts=4 sta ai noet:

TARGET :=


SRCS :=
CFGS :=


CROSS_TOOL :=
CFLAGS := -Wall -Wextra -O2 -I.
LDFLAGS :=


ifdef V
	ifeq ("$(origin V)", "command line")
		BUILD_VERBOSE = $(V)
		Q =
	endif
endif

ifndef BUILD_VERBOSE
	BUILD_VERBOSE = 0
	Q = @
endif

export BUILD_VERBOSE Q


## =============================================

AS = $(CROSS_TOOL)as
CC = $(CROSS_TOOL)gcc
LD = $(CROSS_TOOL)ld
OD = $(CROSS_TOOL)objdump
OC = $(CROSS_TOOL)objcopy


CSRCS := $(filter %.c, $(SRCS))
CXXSRCS := $(filter %.cpp, $(SRCS))
CCSRCS := $(filter %.cc, $(SRCS))
OBJS := $(CSRCS:.c=.o) $(CXXSRCS:.cpp=.o) $(CCSRCS:.cc=.o)
DEPS := $(CSRCS:.c=.d) $(CXXSRCS:.cpp=.d) $(CCSRCS:.cc=.d)



all : $(TARGET)



$(TARGET) : $(OBJS)
ifeq ($(BUILD_VERBOSE),0)
	@echo "Linking.......	[$@]" 
endif
	$(Q)$(CC) -o $@ $(CFLAGS) $^ $(LDFLAGS)

.c.o :
ifeq ($(BUILD_VERBOSE),0)
	@echo "Compiling.....	[$<]" 
endif
	$(Q)$(CC) $(CFLAGS) -c -o $@ $<

.cpp.o :
ifeq ($(BUILD_VERBOSE),0)
	@echo "Compiling.....	[$<]" 
endif
	$(Q)$(CC) $(CFLAGS) -c -o $@ $<

.cc.o :
ifeq ($(BUILD_VERBOSE),0)
	@echo "Compiling.....	[$<]" 
endif
	$(Q)$(CC) $(CFLAGS) -c -o $@ $<



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
				-o -name '*.bak' -o -name '#*#' -o -name '.*.orig' \
				-o -name '*.org' -o -name '.*.rej' \
				-o -name '*%' -o -name 'core' \) \
				-type f -print | xargs rm -f



%.d : %.c
ifeq ($(BUILD_VERBOSE),0)
	@echo "Making dep.....	[$<]" 
endif
	$(Q)set -e; b=`basename $< .c`; d=`dirname $<`; rm -f $@;  \
	$(CC) -MM $(CFLAGS) $< > $@.$$$$;       \
	sed "s,\($${b}\)\.o[ :]*,$${d}/\1.o $${d}/\1.d : ,g" < $@.$$$$ > $@; \
	[ -s $@ ] && rm -f $@.$$$$

%.d : %.cpp
ifeq ($(BUILD_VERBOSE),0)
	@echo "Making dep.....	[$<]" 
endif
	$(Q)set -e; b=`basename $< .cpp`; d=`dirname $<`; rm -f $@;  \
	$(CC) -MM $(CFLAGS) $< > $@.$$$$;       \
	sed "s,\($${b}\)\.o[ :]*,$${d}/\1.o $${d}/\1.d : ,g" < $@.$$$$ > $@; \
	[ -s $@ ] && rm -f $@.$$$$

%.d : %.cc
ifeq ($(BUILD_VERBOSE),0)
	@echo "Making dep.....	[$<]" 
endif
	$(Q)set -e; b=`basename $< .cc`; d=`dirname $<`; rm -f $@;  \
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
