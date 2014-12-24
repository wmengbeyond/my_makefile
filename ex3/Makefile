TARGET = my_app
SRCS = $(wildcard *.c)
OBJS = $(patsubst %.c, %.o, $(SRCS))
CC = gcc
INCLUDE = -I/XXX
LIBS = -L/XXX
CFLAGS = -g -Wall -O0

$(TARGET) : $(OBJS)
#	$(CC) $^ -o $@ $(INCLUDE) $(LIBS) 
	$(CC) $^ -o $@

%.o : %.c
	$(CC) -c $< $(CFLAGS)

.PHONY:clean
clean:
	rm $(TARGET) $(OBJS)
