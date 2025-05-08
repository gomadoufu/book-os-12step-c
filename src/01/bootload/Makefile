TOOLS = ../../../tools

ARCH = h8300-elf
ARCHHYPHEN = h8300-elf-
ADDNAME= $(ARCHHYPHEN)

BINUTILSDIR = binutils-2.21
BINUTILSBIN = $(TOOLS)/$(BINUTILSDIR)/bin

GCCDIR = gcc-14.2.0-h8300
GCCBIN = $(TOOLS)/$(GCCDIR)/bin

BIN = $(BINUTILSBIN)

AR = $(BIN)/$(ADDNAME)ar
AS = $(BIN)/$(ADDNAME)as
CC = $(GCCBIN)/$(ADDNAME)gcc
LD = $(BIN)/$(ADDNAME)ld
NM = $(BIN)/$(ADDNAME)nm
OBJCOPY = $(BIN)/$(ADDNAME)objcopy
OBJDUMP = $(BIN)/$(ADDNAME)objdump
RANLIB = $(BIN)/$(ADDNAME)ranlib
STRIP = $(BIN)/$(ADDNAME)strip

H8WRITE = $(TOOLS)/kz_h8write/kz_h8write

H8WRITE_SERDEV = /dev/tty.usbserial-FTRTK8FG

OBJS = vector.o startup.o main.o
OBJS += lib.o serial.o

TARGET = kzload

CFLAGS = -Wall -mh -nostdlib -nostdinc -fno-builtin
CFLAGS += -I.
CFLAGS += -g
CFLAGS += -Os
CFLAGS += -DKZLOAD

LDFLAGS = -static -T ld.scr -L.

.SUFFIXES: .c .o
.SUFFIXES: .s .o

all: $(TARGET)

$(TARGET): $(OBJS)
	$(CC) $(OBJS) -o $(TARGET) $(CFLAGS) $(LFLAGS)
	cp $(TARGET) $(TARGET).elf
	$(STRIP) $(TARGET)

.c.o: $<
	$(CC) -c $(CFLAGS) $<

.s.o: $<
	$(CC) -c $(CFLAGS) $<

$(TARGET).mot: $(TARGET)
	$(OBJCOPY) -O srec $(TARGET) $(TARGET).mot

image: $(TARGET).mot

write:$(TARGET).mot
	$(H8WRITE) -3069 -f20 $(TARGET).mot $(H8WRITE_SERDEV)

clean:
	rm -f $(OBJS) $(TARGET) $(TARGET).elf $(TARGET).mot

