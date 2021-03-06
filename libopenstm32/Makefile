##
## This file is part of the libopenstm32 project.
##
## Copyright (C) 2009 Uwe Hermann <uwe@hermann-uwe.de>
##
## This program is free software: you can redistribute it and/or modify
## it under the terms of the GNU General Public License as published by
## the Free Software Foundation, either version 3 of the License, or
## (at your option) any later version.
##
## This program is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty of
## MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
## GNU General Public License for more details.
##
## You should have received a copy of the GNU General Public License
## along with this program.  If not, see <http://www.gnu.org/licenses/>.
##

DESTDIR		?= library
LIBNAME		= libopenstm32

PREFIX	?= arm-none-eabi-
# PREFIX		?= arm-elf-
CC		= $(PREFIX)gcc
AR		= $(PREFIX)ar
CFLAGS		= -O3 -g -Wall -Wextra -Winline -I. -fno-common \
		  -mcpu=cortex-m3 -mthumb -Wstrict-prototypes
# ARFLAGS	= rcsv
ARFLAGS		= rcs
OBJS		= rcc.o gpio.o usart.o adc.o spi.o flash.o nvic.o \
		  rtc.o i2c.o dma.o systick.o init.o crc32.o

# Be silent per default, but 'make V=1' will show all compiler calls.
ifneq ($(V),1)
Q := @
endif

ifeq ($(RAM),1)
	CFLAGS += -D_LINK_RAM_
endif

all: $(LIBNAME).a 
doc: README

doc: README mainpage.dox Doxyfile Doxyfile.tmpl
	doxygen

mainpage.doc: README

doc_install: doc
	install -d $(DESTDIR)
	cp -r doc $(DESTDIR)/$(LIBNAME)



$(LIBNAME).a: $(OBJS)
	@printf "  AR      $(subst $(shell pwd)/,,$(@))\n"
	$(Q)$(AR) $(ARFLAGS) $@ $^

%.d: %.o

%.o: %.c Makefile
	@printf "  CC      $(subst $(shell pwd)/,,$(@))\n"
	$(Q)$(CC) $(CFLAGS) -o $@ -c $<
	@$(CC) $(CFLAGS) -MD -c $< -o $@

README: README.texi
	makeinfo --no-headers --html $< > $@

clean:
	@printf "  CLEAN   $(subst $(shell pwd)/,,$(OBJS))\n"
	$(Q)rm -f *.o
	@printf "  CLEAN   $(LIBNAME).a\n"
	$(Q)rm -f $(LIBNAME).a
	$(Q)rm -f README
	$(Q)rm -f *.d
	$(Q)rm -rf doc/

.PHONY: clean doc

-include $(OBJS:.o=.d)
