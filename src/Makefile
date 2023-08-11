#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# Makefile to create the microcontroller applications for the Embedded Course
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#       <FILE>.i - Builds <FILE>.i preprocessed output of <FILE>.c
#       <FILE>.asm - Builds <FILE>.asm assembly output of <FILE>.C
#       <FILE>.o - Buids <FILE>.o object file for <FILE>.c
#       compile-all - Compiles all object files, but DO NOT link
#      build - Compile all object files and link into a final executable.
#      all
#      clean - Removes all generated files
#
# Platform Overrides:
#      PLATFORM - Target PLATFFORM (HOST, MSP432)
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
PLATFORM = HOST

# Architectures Specific Flags
LINKER_FILE = ../msp432p401r.lds

# Compiler Flags and Defines
ifeq ($(PLATFORM),HOST)
	CC = gcc
	CFLAGS = -DHOST
else ifeq($(PLATFORM),MSP432)
	CC = arm-none-eabi-gcc
	CPU = cortex-m4
	ARCH = thumb
	SPECS = nosys.specs
	CFLAGS = -DMSP432 -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS)
else
	echo "'$(PLATFORM) : Unsupported PLATFOM. Must be HOST or MSP432"
	exit 1
endif

LD =
LDFLAGS = 
CPPFLAGS = -E
ASMFLAGS = -S
OBJDUMP = objdump

TARGET = c1m2

OBJS = $(SRCS:.c=.o)
PP_FILES = $(SRCS:.c=.oi)
ASM_FILES = $(SRCS:.c=.asm) $(SRCS:.c=.s)

%.o : %.c
	$(CC) -c $< $(CFLAGS) -o $@

%.i : %.c
	$(CC) $(CPPLAGS) $< $(CFLAGS) -o $@

%.asm : %.o
	$(CC) -$(ASMLAGS) $< $(CFLAGS) -o $@
	$(OBJS) $@


.PHONY: compile-all build clean

compile-all:

build:  $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(LDFLAGS) -o $(TARGET).out

clean:
	rm -f $(OBJS) $(PP_FILES) $(ASM_FILES) $(TARGET).out $(TARGET).map
