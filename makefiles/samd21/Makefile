##########################################################################################################################
# File automatically-generated by tool: [projectgenerator] version: [3.7.1] date: [Thu Jul 23 00:07:34 BST 2020]
##########################################################################################################################

# ------------------------------------------------
# Generic Makefile (based on gcc)
#
# ChangeLog :
#	2017-02-10 - Several enhancements + project update mode
#   2015-07-22 - first version
# ------------------------------------------------

######################################
# target
######################################
TARGET = uCNC

######################################
# building variables
######################################
# debug build?
DEBUG ?= 0
# optimization
ifeq ($(DEBUG), 1)
OPT = -Og
else
OPT = -Os
endif


#######################################
# paths
#######################################
# Build path
BUILD_DIR = build

######################################
# helper
######################################
rwildcard=$(wildcard $1$2) $(foreach d,$(wildcard $1*),$(call rwildcard,$d/,$2))

######################################
# source
######################################
# C sources
C_SOURCES = $(call rwildcard,./,*.c)
C_SOURCES += $(call rwildcard,../../uCNC/src,*.c)

# ifdef USE_USB
# C_SOURCES += ../../tinyusb/tusb_descriptors.c
# C_SOURCES += $(call rwildcard,../../tinyusb/src/,*.c)
# endif

# ASM sources
#ASM_SOURCES =  \
#startup_stm32f103xb.s


#######################################
# binaries
#######################################
PREFIX = arm-none-eabi-
# The gcc compiler bin path can be either defined in make command via GCC_PATH variable (> make GCC_PATH=xxx)
# either it can be added to the PATH environment variable.
ifdef GCC_PATH
CC = $(GCC_PATH)/$(PREFIX)gcc
AS = $(GCC_PATH)/$(PREFIX)gcc -x assembler-with-cpp
CP = $(GCC_PATH)/$(PREFIX)objcopy
SZ = $(GCC_PATH)/$(PREFIX)size
else
CC = $(PREFIX)gcc
AS = $(PREFIX)gcc -x assembler-with-cpp
CP = $(PREFIX)objcopy
SZ = $(PREFIX)size
endif
HEX = $(CP) -O ihex
BIN = $(CP) -O binary -S
 
#######################################
# CFLAGS
#######################################
# cpu
CPU = -mcpu=cortex-m0plus
BOARD ?= BOARD_MZERO

# fpu
# NONE for Cortex-M0/M0+/M3

# float-abi

#interface 0-USART 1-USB

# mcu
MCU = $(CPU) -mthumb $(FPU) $(FLOAT-ABI)

# macros for gcc
# AS defines
AS_DEFS = 

# C defines
BUILD_OPTIONS ?=

C_DEFS =  $(BUILD_OPTIONS)\
-DSAMD21 \
-DCFG_TUSB_MCU=OPT_MCU_SAMD21 \
-D__SAMD21E18A__ \
-DBOARD=$(BOARD)

# AS includes
AS_INCLUDES = 

# C includes
C_HEADERS = $(call rwildcard,./,*.h)
# ifdef USE_USB
# C_HEADERS += ../../tinyusb/tusb_config.h
# C_HEADERS += $(call rwildcard,../../tinyusb/src/,*.h)
# endif


# C includes
C_INCLUDES =  \
-I"./" \
-Idrivers/ \
-Iasf/ \
-I"../../uCNC/" \
-I"../../uCNC/src/"\
-I"../../uCNC-tinyusb/src"

C_INCLUDES += $(foreach d,$(C_HEADERS),$(addsuffix $(dir $(d)),"-I"))

# compile gcc flags
ASFLAGS = $(MCU) $(AS_DEFS) $(AS_INCLUDES) $(OPT) -Wall -fdata-sections -ffunction-sections -nostdlib -fno-threadsafe-statics -fno-rtti -fno-exceptions

CFLAGS = $(MCU) $(C_DEFS) $(C_INCLUDES) $(OPT) -Wall -fdata-sections -ffunction-sections -nostdlib -fno-threadsafe-statics -fno-rtti -fno-exceptions

ifeq ($(DEBUG), 1)
CFLAGS += -g3 -ggdb3 -gdwarf-2
endif


# Generate dependency information
CFLAGS += -MMD -MP -MF"$(@:%.o=%.d)"


#######################################
# LDFLAGS
#######################################
# link script
LDSCRIPT = samd21.ld

# libraries
LIBS = -lc -lm -lnosys -static-libgcc -lgcc
LIBDIR = 
LDFLAGS = $(MCU) -specs=nano.specs -T$(LDSCRIPT) $(LIBDIR) $(LIBS) -Wl,-Map=$(BUILD_DIR)/$(TARGET).map,--cref -Wl,--gc-sections

# default action: build all
all: $(BUILD_DIR)/$(TARGET).elf $(BUILD_DIR)/$(TARGET).hex $(BUILD_DIR)/$(TARGET).bin


#######################################
# build the application
#######################################
# list of objects
OBJECTS = $(addprefix $(BUILD_DIR)/,$(notdir $(C_SOURCES:.c=.o)))
vpath %.c $(sort $(dir $(C_SOURCES)))
# list of ASM program objects
OBJECTS += $(addprefix $(BUILD_DIR)/,$(notdir $(ASM_SOURCES:.s=.o)))
vpath %.s $(sort $(dir $(ASM_SOURCES)))

$(BUILD_DIR)/%.o: %.c | $(BUILD_DIR) 
	$(CC) -c $(CFLAGS) -Wa,-a,-ad,-alms=$(BUILD_DIR)/$(notdir $(<:.c=.lst)) $< -o $@

$(BUILD_DIR)/%.o: %.s | $(BUILD_DIR)
	$(AS) -c $(CFLAGS) $< -o $@

$(BUILD_DIR)/$(TARGET).elf: $(OBJECTS)
	$(CC) $(OBJECTS) $(LDFLAGS) -o $@
	$(SZ) $@

$(BUILD_DIR)/%.hex: $(BUILD_DIR)/%.elf | $(BUILD_DIR)
	$(HEX) $< $@
	
$(BUILD_DIR)/%.bin: $(BUILD_DIR)/%.elf | $(BUILD_DIR)
	$(BIN) $< $@	
	
$(BUILD_DIR):
	mkdir $@		

#######################################
# clean up
#######################################
clean:
	-rm -fR $(BUILD_DIR)
  
#######################################
# dependencies
#######################################
-include $(wildcard $(BUILD_DIR)/*.d)

# *** EOF ***