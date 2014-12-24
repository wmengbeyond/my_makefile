 #全局Makefile文件

 # * --------------------使用说明--------------------
 # 此Makefile执行时有两种模式
 # 一、非递归make模式
 #    自动编译当前目录下的所有.c文件以并分别链接生成对应的.o文件和可执行文件。
 #    给simple_mode赋任意值，然后包含此Makefile文件即可开启此模式。
 # 二、递归make模式
 #    会在当前目录的所有子目录下分别执行make命令
 #    给recursive_mode赋任意值，然后包含此Makefile文件即可开启此模式。
 # 其它变量说明见各自的注释

 # * --------------------整体目标--------------------

 # 三个虚目标
 # all        编译链接所有文件
 # debug      用来辅助测试Makefile，比如打印一些变量等等
 # clean      清理所有文件

 .PHONY : all clean debug

 all ::

 debug ::

 clean ::

 # * --------------------CFLAGS和LDFLAGS--------------------

 # ** 标准变量CFLAGS和LDFLAGS设置

 # 打开警告和调试信息选项
 CFLAGS += -Wall -g

 # ** 添加对gtk程序的编译链接支持

 # 自定义变量:
 #    gtk                相当于gtk2
 #    gtk2               按照gtk2的环境来设置CFLAGS
 #    gtk3               按照gtk3的环境来设置CFLAGS

 # 生成gtk程序，默认为gtk2
 ifdef gtk
 CFLAGS += $(shell pkg-config --cflags --libs gtk+-2.0)
 endif
 ifdef gtk2
 CFLAGS += $(shell pkg-config --cflags --libs gtk+-2.0)
 endif
 ifdef gtk3
 CFLAGS += $(shell pkg-config --cflags --libs gtk+-3.0)
 endif

 # * --------------------简单make模式(不递归)--------------------

 # 自定义变量:
 #    simple_mode                开启简单make模式
 #    sources_exclude            忽略的源文件列表
 #    objects_exclude            忽略的目标文件列表
 #    programs_exclude           忽略的程序文件列表(即最终的可执行文件)
 #    rmobj                      删除的文件列表
 #    rmobj_exclude              忽略的删除文件列表

 # 说明：
 #
 # 1、如果一个源文件添加到了$(sources_exclude)变量中，那么它对应的目标文
 # 件和最终的可执行文件会被自动添加到$(objects_exclude)和
 # $(programs_exclude)中
 #
 # 2、$(rmobj_exclude)是指这样一个文件列表，原本应该被删除的文件(例如某
 # 个可执行文件)，但不想让它在执行make clean时被自动删除


 # --------------------对当前目录下的普通文件执行make--------------------
 sources_all = $(wildcard *.c)
 sources = $(filter-out $(sources_exclude), $(sources_all))

 objects_all = $(patsubst %.c,%.o,$(sources))
 objects += $(filter-out $(objects_exclude), $(objects_all))

 programs_all = $(patsubst %.o,%,$(objects))
 programs += $(filter-out $(programs_exclude), $(programs_all))

 rmobj += $(wildcard *~)

 ifdef simple_mode
 all :: $(programs)

debug ::
	echo sources_all: $(sources_all)
	echo sources_exclude: $(sources_exclude)
	echo sources: $(sources)
	echo objects_all: $(objects_all)
	echo objects_exclude: $(objects_exclude)
	echo objects: $(objects)
	echo programs_all: $(programs_all)
	echo programs_exclude: $(programs_exclude)
	echo programs: $(programs)
	echo rmobj: $(rmobj)

clean ::
	-rm -f $(programs_all) $(objects_all) $(rmobj)
endif

# * --------------------递归make模式--------------------

#自定义变量:
#    recursive_mode        是否开启递归make模式
#    subdirs_exclude       被忽略的文件夹列表，即不会进入这个文件夹执行递归make

# 判断是否存在子目录以及在子文件夹里执行make
ifdef recursive_mode
	subdirs_all := $(sort $(subst /,,$(dir $(wildcard */*))))
	subdirs = $(filter-out $(subdirs_exclude), $(subdirs_all))
all :: $(subdirs)
         @for subdir in $(subdirs); do \
			 $(MAKE) -C $subdir; \
		 done

debug ::
	echo subdirs_all: $(subdirs_all)
	echo subdirs_exclude: $(subdirs_exclude)
	echo subdirs: $(subdirs)

clean ::
	@for subdir in $(subdirs); do \
		$(MAKE) -C $subdir clean; \
	done
endif

# * --------------------添加对flymake的支持--------------------

check-syntax:
	gcc $(CFLAGS) -o /tmp/flymake-nul -S ${CHK_SOURCES}
