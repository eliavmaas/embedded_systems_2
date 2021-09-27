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
#
#------------------------------------------------------------------------------
# Makefile for Embedded System Coursera Course - Assignment 2. By specifying platform
# (Host or MSP432) different compilers and flags are used. There is also the ability 
# to make specific output files described in the build targets below.
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <FILE>.o - Builds <FILE>.o object file
#	   <FILE>.d - Builds <FILE>.d dependency file
#	   <FILE>.i - Builds <FILE>.i implementation file
#	   <FILE>.asm, - Builds <FILE>.asm assembly file
#	   compile-all - Builds all source files but does not link
#      build - Builds and links all source files
#      all - Same as build
#      clean - removes all generated files#
# Platform Overrides:
#      CPU - cortex-m4 or host
#      ARCH - armv7e-m or host
#
#------------------------------------------------------------------------------
include sources.mk

TARGET = c1m2

ifeq ($(PLATFORM),MSP432)
	CC = arm-none-eabi-gcc
	CPU = cortex-m4
	ARCH = armv7e-m
	LINKER_FILE = ../msp432p401r.lds
	CFLAGS = -mcpu=$(CPU) -march=$(ARCH) \
			-mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 \
			--specs=nosys.specs -DMSP432
	LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
else
	CC = gcc
	CPU = x86_64
	ARCH := $(shell arch)
	CFLAGS = -DHOST
	LDFLAGS = -Wl,-Map=$(TARGET).map
endif


LD = arm-none-eabi-ld

CPPFLAGs = -Wall -Werror -g -O0 -std=c99

OBJS = $(SOURCES:.c=.o)
DEPS = $(SOURCES:.c=.d)
IMP = $(SOURCES:.c=.i)
ASM = $(SOURCES:.c=.asm) $(TARGET.out=.asm)

%.o : %.c
	$(CC) $(includes) -c $< $(CFLAGS) $(CPPFLAGs) -o $@

%.d : %.c
	$(CC) $(includes) -M $< $(CFLAGS) $(CPPFLAGs) -o $@

%.i : %.c
	$(CC) $(includes) -E $< $(CFLAGS) $(CPPFLAGs) -o $@

%.asm : %.c
	$(CC) $(includes) -S $< $(CFLAGS) $(CPPFLAGs) -o $@

%.asm : %.out
	objdump -d $< > $@


.PHONY: compile-all
compile-all: $(OBJS) $(DEPS)

.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out

$(TARGET).out: $(OBJS) $(DEPS) $(IMP)
	$(CC) $(OBJS) $(CFLAGS) $(CPPFLAGs) $(LDFLAGS) -o $@
	size $@

.PHONY: clean
clean:
# 	  rm -f $(OBJS) $(DEPS) $(ASM) $(IMP) $(TARGET).out $(TARGET).map
	rm -f *.o *.i *.d *.out *.map *.asm