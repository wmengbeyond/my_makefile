#声明伪目标，防止Makefile去生成all等
.PHONY = all install clean

#定义路径变量，所有.c文件和所有非公开的.h应该放在src下，所有需要的.a文件放在lib
#下，所有公开的.h（比如生成库文件的时候）或者多个.c公用的.h放在include文件夹下
#global directory defined
TOPDIR = $(shell pwd)
SRCDIR         = $(TOPDIR)/src
LIBDIR = $(TOPDIR)/lib
OBJECTDIR = $(TOPDIR)/build
INCLUDEDIR = $(TOPDIR)/include

#定义交叉编译环境变量，当需要编译arm/mips等平台应用程序/库的时候修改它
#cross compile tools defined 
CROSS_COMPILE = 
AS = $(CROSS_COMPILE)as
LD = $(CROSS_COMPILE)ld
CC = $(CROSS_COMPILE)gcc
CPP = $(CC) -E
AR = $(CROSS_COMPILE)ar
NM = $(CROSS_COMPILE)nm
STRIP = $(CROSS_COMPILE)strip
RANLIB     = $(CROSS_COMPILE)ranlib

#本机相关的命令，一般无需修改
#local host tools defined
CP        := cp
RM        := rm
MKDIR    := mkdir
SED        := sed
FIND    := find
MKDIR    := mkdir
XARGS    := xargs

#增加对其他子目录中的Makefile文件自动调用 make base
#SUBDIRS = $(TOPDIR)/base $(TOPDIR)/base1
SUBDIRS = $(TOPDIR)/base
MAKE = make
default:base
usr:
	@for n in $(SUBDIRS); do $(MAKE) -C $$n ; done

#目标名称，这里我们给出了三种常用的目标格式：目标文件，静态库和共享库
#target name
TARGETMAIN     = testmk

#所有源码文件的路径被放入SOURCEDIRS，所有.c源码文件（含路径）放入SOURCES
#.c .o and .d files defined
VPATH             = $(shell ls -AxR $(SRCDIR)|grep ":"|grep -v "\.svn"|tr -d ':')
	SOURCEDIRS    = $(VPATH)
	SOURCES     = $(foreach subdir,$(SOURCEDIRS),$(wildcard $(subdir)/*.c))

#所有目标文件.o（含路径）放入BUILDOBJS，注意它们的路径已经是build了。
SRCOBJS             = $(patsubst %.c,%.o,$(SOURCES))
BUILDOBJS = $(subst $(SRCDIR),$(OBJECTDIR),$(SRCOBJS))

#所有.d依赖文件放入DEPS
DEPS            = $(patsubst %.o,%.d,$(BUILDOBJS))

#注意-MD，是为了生成.d文件后，构造对.h的依赖
#external include file define
CFLAGS    = -O2 -Wall -MD $(foreach dir,$(INCLUDEDIR),-I$(dir))
ARFLAGS = rc

#special parameters for app
CFLAGS    +=

#LDFLAGS指明所有-llibxx，libxx.a应该放到lib下，当然也可以添加.so。Xlinker是为了
#在提供多个.a时，未知它们之间的依赖顺序时，自动查找依赖顺序
#c file compile parameters and linked libraries
CPPFLAGS = 
LDFLAGS     =
XLDFLAGS = -Xlinker "-(" $(LDFLAGS) -Xlinker "-)"
LDLIBS         += -L $(LIBDIR) 

#添加指定需要链接的库文件名，这里示例是libhello.so 或者libhello.a
#LLIBNAME = -llibname
LLIBNAME = -lhello

#如果要生成.a或者.so，那么不要将main函数所在的.c放入src。另外添加$(TARGETLIBS) 
#或$(TARGETSLIBS)到all中
#defaut target:compile the currrent dir file and sub dir 
all: $(TARGETMAIN)

#for .h header files dependence
-include $(DEPS)

$(TARGETMAIN) :$(BUILDOBJS)
	    @$(CC) $(subst $(SRCDIR),$(OBJECTDIR),$^) $(CPPFLAGS) $(CFLAGS) $(XLDFLAGS) -o $@ $(LDLIBS) $(LLIBNAME)
		    @$(STRIP) --strip-unneeded $(TARGETMAIN)

#这里是Makefile的核心，根据%中的内容，查找src路径下对应的.c，注意到$@和$<自动
#变量的取值，首先查看路径build/xx是否存在，不存在则创建，然后我们尝试将$@中的src
#替换为build，这样所有的.o和.d都将被创建到对应的build下了。
$(OBJECTDIR)%.o: $(SRCDIR)%.c
	    @[ ! -d $(dir $(subst $(SRCDIR),$(OBJECTDIR),$@)) ] & $(MKDIR) -p $(dir $(subst $(SRCDIR),$(OBJECTDIR),$@))
		    @$(CC) $(CPPFLAGS) $(CFLAGS) -o $(subst $(SRCDIR),$(OBJECTDIR),$@) -c $<

#添加安装的路径
#install:

clean:
	    @$(FIND) $(OBJECTDIR) -name "*.o" -o -name "*.d" | $(XARGS) $(RM) -f
		    @$(RM) -f $(TARGETMAIN)
