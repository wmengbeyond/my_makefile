.PHONY = all install clean

TOPDIR = $(shell pwd)
SRCDIR = $(TOPDIR)
PARENTDIR = ..
LIBDIR = $(PARENTDIR)/lib
OBJECTDIR = $(PARENTDIR)/base_build
INCLUDEDIR = $(PARENTDIR)/include

CP        := cp
RM        := rm
MKDIR    := mkdir
SED        := sed
FIND    := find
MKDIR    := mkdir
XARGS    := xargs

TARGETLIBS     = $(PARENTDIR)/lib/libhello.a
TARGETSLIBS    = $(PARENTDIR)/lib/libhello.so

VPATH             = $(shell ls -AxR $(SRCDIR)|grep ":"|grep -v "\.git"|tr -d ':')
	SOURCEDIRS    = $(VPATH)
	SOURCES     = $(foreach subdir,$(SOURCEDIRS),$(wildcard $(subdir)/*.c))

SRCOBJS             = $(patsubst %.c,%.o,$(SOURCES))
BUILDOBJS = $(subst $(SRCDIR),$(OBJECTDIR),$(SRCOBJS))

DEPS            = $(patsubst %.o,%.d,$(BUILDOBJS))

CFLAGS    = -O2 -Wall -MD $(foreach dir,$(INCLUDEDIR),-I$(dir))
ARFLAGS = rc

CFLAGS    +=

CPPFLAGS = 
LDFLAGS     =
XLDFLAGS = -Xlinker "-(" $(LDFLAGS) -Xlinker "-)"
LDLIBS         += -L $(LIBDIR) 

all: $(TARGETLIBS)
all: $(TARGETSLIBS)

-include $(DEPS)


$(TARGETLIBS) :$(BUILDOBJS)
	    @$(AR) $(ARFLAGS) $@ $(BUILDOBJS)
		    @$(RANLIB) $@

$(TARGETSLIBS) :$(BUILDOBJS)
	    @$(CC) -shared $(subst $(SRCDIR),$(OBJECTDIR),$^) $(CPPFLAGS) $(CFLAGS) $(XLDFLAGS) -o $@ $(LDLIBS)

$(OBJECTDIR)%.o: $(SRCDIR)%.c
	    @[ ! -d $(dir $(subst $(SRCDIR),$(OBJECTDIR),$@)) ] & $(MKDIR) -p $(dir $(subst $(SRCDIR),$(OBJECTDIR),$@))
		    @$(CC) $(CPPFLAGS) $(CFLAGS) -o $(subst $(SRCDIR),$(OBJECTDIR),$@) -c $<

clean:
	    @$(FIND) $(OBJECTDIR) -name "*.o" -o -name "*.d" | $(XARGS) $(RM) -f
		    @$(RM) -f $(TARGETLIBS) $(TARGETSLIBS)
