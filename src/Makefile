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
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <FILE>.o - Builds <FILE>.o object file
#      build - Builds and links all source files
#      all - Same as build
#      clean - removes all generated files#
# Platform Overrides:
#      CPU - ARM Cortex Architecture (cortex-m0plus, cortex-m4)
#      ARCH - ARM Architecture (arm, thumb)
#      SPECS - Specs file to give the linker (nosys.specs, nano.specs)
#
#------------------------------------------------------------------------------
include sources.mk

TARGET = c1m2

# Platform Overrides

# Architectures Specific Flags

# -T msp432p401r.lds
ifeq ($(PLATFORM),MSP432)
	CC = arm-none-eabi-gcc
	CPU = cortex-m4
	ARCH = armv7e-m
	LINKER_FILE = msp432p401r.lds
	CFLAGS = -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS) \
			-mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 \
			-specs=nosys.specs -DMSP432
	LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
else
	CC = gcc
	CPU = x86_64
	ARCH := $(shell arch)
	CFLAGS = -DHOST
	LDFLAGS = -Wl,-Map=$(TARGET).map
endif



# Compiler Flags and Defines

LD = arm-none-eabi-ld
# LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
# CFLAGS = -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS) -Wall
CPPFLAGs = -Wall -Werror -g -O0 -std=c99

OBJS = $(SOURCES:.c=.o)

%.o : %.c
	$(CC) -I $(includes) -c $< $(CFLAGS) $(CPPFLAGs) -o $@

# %.i : %.c
# 	$(CC) -c $< $(CFLAGS) $(CPPFLAGs) -o $@

.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(CPPFLAGs) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET).out $(TARGET).map