## vim:set ft=make ts=4 sw=4 sts=4 sta ai noet:

TARGET :=

FILE_EXT := none


SRCS :=
CFGS :=



CROSS_COMPILE ?=
CFLAGS := -Wall -Wextra -O2 -I.
LDFLAGS := -lstdc++



CSRCS := $(filter %.c, $(SRCS))
CXXSRCS := $(filter %.cpp, $(SRCS))
CCSRCS := $(filter %.cc, $(SRCS))
OBJS := $(CSRCS:%.c=%.${FILE_EXT}.o) $(CXXSRCS:%.cpp=%.${FILE_EXT}.o) $(CCSRCS:%.cc=%.${FILE_EXT}.o)
DEPS := $(CSRCS:%.c=%.${FILE_EXT}.depend) $(CXXSRCS:%.cpp=%.${FILE_EXT}.depend) $(CCSRCS:%.cc=%.${FILE_EXT}.depend)



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

HOSTCC	= gcc
HOSTCXX	= g++

AS		= $(CROSS_COMPILE)as
CC		= $(CROSS_COMPILE)gcc
CXX		= $(CROSS_COMPILE)g++
CPP		= $(CC) -E
LD		= $(CROSS_COMPILE)ld
OD		= $(CROSS_COMPILE)objdump
OC		= $(CROSS_COMPILE)objcopy
AR      = $(CROSS_COMPILE)ar
NM      = $(CROSS_COMPILE)nm
STRIP   = $(CROSS_COMPILE)strip


export HOSTCC HOSTCXX
export CROSS_COMPILE
export AS CC CXX CPP LD OD OC AR NM STRIP



all : $(TARGET)



$(TARGET) : $(OBJS)
ifeq ($(BUILD_VERBOSE),0)
	@echo "Linking.......	[$@]" 
endif
	$(Q)$(CC) -o $@ $(CFLAGS) $^ $(LDFLAGS)

%.${FILE_EXT}.o : %.c
ifeq ($(BUILD_VERBOSE),0)
	@echo "Compiling.....	[$<]" 
endif
	$(Q)$(CC) $(CFLAGS) -c -o $@ $<

%.${FILE_EXT}.o : %.cpp
ifeq ($(BUILD_VERBOSE),0)
	@echo "Compiling.....	[$<]" 
endif
	$(Q)$(CXX) $(CFLAGS) -c -o $@ $<

%.${FILE_EXT}.o : %.cc
ifeq ($(BUILD_VERBOSE),0)
	@echo "Compiling.....	[$<]" 
endif
	$(Q)$(CXX) $(CFLAGS) -c -o $@ $<



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
	-@find . \( -name '*.[oas]' -o -name '.*.depend' -o -name '.*.depend.*' \) \
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



%.${FILE_EXT}.depend : %.c
ifeq ($(BUILD_VERBOSE),0)
	@echo "Making dep.....	[$<]" 
endif
	$(Q)set -e; b=`basename $< .c`; d=`dirname $<`; rm -f $@;  \
	$(CC) -MM $(CFLAGS) $< > $@.$$$$;       \
	sed "s,\($${b}\)\.o[ :]*,$${d}/\1.o $${d}/\1.${FILE_EXT}.depend : ,g" < $@.$$$$ > $@; \
	[ -s $@ ] && rm -f $@.$$$$

%.${FILE_EXT}.depend : %.cpp
ifeq ($(BUILD_VERBOSE),0)
	@echo "Making dep.....	[$<]" 
endif
	$(Q)set -e; b=`basename $< .cpp`; d=`dirname $<`; rm -f $@;  \
	$(CC) -MM $(CFLAGS) $< > $@.$$$$;       \
	sed "s,\($${b}\)\.o[ :]*,$${d}/\1.o $${d}/\1.${FILE_EXT}.depend : ,g" < $@.$$$$ > $@; \
	[ -s $@ ] && rm -f $@.$$$$

%.${FILE_EXT}.depend : %.cc
ifeq ($(BUILD_VERBOSE),0)
	@echo "Making dep.....	[$<]" 
endif
	$(Q)set -e; b=`basename $< .cc`; d=`dirname $<`; rm -f $@;  \
	$(CC) -MM $(CFLAGS) $< > $@.$$$$;       \
	sed "s,\($${b}\)\.o[ :]*,$${d}/\1.o $${d}/\1.${FILE_EXT}.depend : ,g" < $@.$$$$ > $@; \
	[ -s $@ ] && rm -f $@.$$$$



doc :
	@-rm -fr html
	@doxygen DOCUMENT/Doxyfile



## unit test를 수행한다.
unittest :
	@TEST_DIR=`find . -type d -name "unittest" -print`; \
	for tdir in $${TEST_DIR}; do make -C $${tdir} || exit 1; done;  \
	for tdir in $${TEST_DIR}; do make -C $${tdir} run; done
	

run : ${TARGET}
	./${TARGET}
