TARGET = my_app
MAIN = main.o
SRCS = $(wildcard *.c)
OBJS = $(patsubst %.c, %.o, $(SRCS))
CC = gcc
INCLUDE = -I/XXX
LIBS = -L/XXX
CFLAGS = -g -Wall -O0
TARGET_LIB = libmylib.a
TARGET_SHARE_LIB = libmylib.so
AR = ar
LD = ld
LDFLAGS = -shared -fpic

$(TARGET) : $(OBJS)
#	$(CC) $^ -o $@ $(INCLUDE) $(LIBS) 
	$(CC) $^ -o $@

#生成库文件
	$(LD) $(LDFLAGS) -o $(TARGET_SHARE_LIB) $(filter-out $(MAIN),$(OBJS))

	$(AR) -rcs $(TARGET_LIB) $(filter-out $(MAIN),$(OBJS))	

%.o : %.c
	$(CC) -c $< $(CFLAGS)

.PHONY:clean
clean:
	rm $(TARGET) $(TARGET_LIB) $(TARGET_SHARE_LIB) $(OBJS)
