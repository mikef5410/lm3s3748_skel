
PROJ_ROOT=../..
RTOS_SOURCE_DIR=$(PROJ_ROOT)/FreeRTOSV6.1.1
LUMINARY_DIR=$(PROJ_ROOT)/stellarisWare
LUMINARY_DRIVER_DIR=$(LUMINARY_DIR)/driverlib
LUMINARY_GR_DIR=$(LUMINARY_DIR)/grlib

TOOLDIR=/opt/cross-arm/arm-bare_newlib_cortex_m3_nommu-eabi/bin
TOOLPREFIX=arm-bare_newlib_cortex_m3_nommu-eabi

CC=$(TOOLDIR)/$(TOOLPREFIX)-gcc
OBJCOPY=$(TOOLDIR)/$(TOOLPREFIX)-objcopy
LDSCRIPT=standalone.ld

# should use --gc-sections but the debugger does not seem to be able to cope with the option.
LINKER_FLAGS=-nostartfiles -Xlinker -oRTOSDemo.axf -Xlinker -M -Xlinker -Map=rtosdemo.map -Xlinker --no-gc-sections

# --eh-frame-hdr

DEBUG=-g
OPTIM=-O0


CFLAGS=$(DEBUG) -I . -I $(RTOS_SOURCE_DIR)/include \
	-I $(RTOS_SOURCE_DIR)/portable/GCC/ARM_CM3 \
		-I $(LUMINARY_DIR) \
		-D GCC_ARMCM3_LM3S102 -D inline=-mthumb -mcpu=cortex-m3 \
		 $(OPTIM) -T$(LDSCRIPT) \
		-D PACK_STRUCT_END=__attribute\(\(packed\)\) -D ALIGN_STRUCT_END=__attribute\(\(aligned\(4\)\)\) -D sprintf=usprintf -D snprintf=usnprintf -D printf=uipprintf \
		-ffunction-sections -fdata-sections

SOURCE=	main.c \
		formike128x128x16.c \
		timertest.c \
		$(LUMINARY_DIR)/utils/ustdlib.c \
		$(LUMINARY_DIR)/utils/uartstdio.c \
		$(LUMINARY_DIR)/utils/cmdline.c \
		$(RTOS_SOURCE_DIR)/list.c \
		$(RTOS_SOURCE_DIR)/queue.c \
		$(RTOS_SOURCE_DIR)/tasks.c \
		$(RTOS_SOURCE_DIR)/portable/GCC/ARM_CM3/port.c \
		$(RTOS_SOURCE_DIR)/portable/MemMang/heap_2.c

LIBS= -L $(LUMINARY_DRIVER_DIR)/gcc -ldriver -L $(LUMINARY_GR_DIR)/gcc -lgr

OBJS = $(SOURCE:.c=.o)

all: RTOSDemo.bin

RTOSDemo.bin : RTOSDemo.axf
	$(OBJCOPY) RTOSDemo.axf -O binary RTOSDemo.bin

RTOSDemo.axf : $(OBJS) startup.o Makefile
	$(CC) $(CFLAGS) $(OBJS) startup.o $(LIBS) $(LINKER_FLAGS)

$(OBJS) : %.o : %.c Makefile FreeRTOSConfig.h
	$(CC) -c $(CFLAGS) $< -o $@

startup.o : startup.c Makefile
	$(CC) -c $(CFLAGS) -O1 startup.c -o startup.o

clean :
	touch Makefile
	rm $(OBJS)




