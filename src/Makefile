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

# Define default PLATFORM
ifndef PLATFORM
  PLATFORM = HOST
endif

# Check PLATFORM validity
ifeq ($(filter HOST MSP432, $(PLATFORM)),)
  ERROR_MSG=$(error '$(PLATFORM)' is not a valid PLATFORM => use HOST or MSP432)
  echo $(ERROR_MSG)
endif

include sources.mk

# Architectures Specific Flags
LINKER_FILE = ../msp432p401r.lds

# Compiler Flags and Defines
ifeq ($(PLATFORM),MSP432)
  CC = arm-none-eabi-gcc
  CPU = cortex-m4
  ARCH = thumb
  SPECS = nosys.specs
  CFLAGS = -DMSP432 -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS) $(INCLUDES)
  SIZEPRG = arm-none-eabi-size
else
  CC = gcc
  CFLAGS = -DHOST $(INCLUDES)
  SIZEPRG = size
endif

LD =
LDFLAGS = 
CPPFLAGS = -E
ASMFLAGS = -S # to produce human readable assembly code
OBJDUMP = objdump

TARGET = c1m2

OBJS = $(SOURCES:.c=.o)
PP_FILES = $(SOURCES:.c=.i)
ASM_FILES = $(SOURCES:.c=.asm) $(SOURCES:.c=.s)

%.o : %.c
	@if echo ${SOURCES} | grep -q $<; then \
		echo "building $@"; \
		$(CC) -c $< $(CFLAGS) -o $@; \
	else \
		echo "$@ is not a valid target for this PLATRFORM : $(PLATFORM)"; \
	fi

%.i : %.c
	@if echo ${SOURCES} | grep -q $<; then \
		echo "building $@"; \
		$(CC) $(CPPFLAGS) $< $(CFLAGS) -o $@; \
	else \
		echo "$@ is not a valid target for this PLATRFORM : $(PLATFORM)"; \
	fi

%.asm : %.c
	@if echo ${SOURCES} | grep -q $<; then \
		echo "building $@"; \
		$(CC) $(ASMFLAGS) $< $(CFLAGS) -o $@; \
	else \
		echo "$@ is not a valid target for this PLATRFORM : $(PLATFORM)"; \
	fi

.PHONY: compile-all build clean

compile-all: $(OBJS)

build:  $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(LDFLAGS) -o $(TARGET).out

clean:
	rm -f *.o *.i *.s *.asm $(TARGET).out $(TARGET).map
