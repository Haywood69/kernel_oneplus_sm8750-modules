# Makefile for use with Android's kernel/build system

KBUILD_OPTIONS += CAMERA_KERNEL_ROOT=$(shell pwd)
KBUILD_OPTIONS += KERNEL_ROOT=$(ROOT_DIR)/$(KERNEL_DIR)
KBUILD_OPTIONS += MODNAME=camera
KBUILD_OPTIONS += KBUILD_EXTRA_SYMBOLS+=$(M)/../mmrm-driver/Module.symvers
KBUILD_OPTIONS += KBUILD_EXTRA_SYMBOLS+=$(M)/../securemsm-kernel/Module.symvers
KBUILD_OPTIONS += KBUILD_EXTRA_SYMBOLS+=$(M)/../synx-kernel/Module.symvers

# Generate into the module's output directory when building with O=; the
# source tree is read-only inside the Android build sandbox.
ifneq ($(O),)
CAMERA_GEN_DIR := $(abspath $(O)/$(M))
else
CAMERA_GEN_DIR := $(shell pwd)
endif
KBUILD_OPTIONS += CAMERA_GEN_DIR=$(CAMERA_GEN_DIR)

all: modules

CAMERA_COMPILE_TIME = $(shell date)
CAMERA_COMPILE_BY = $(shell whoami | sed 's/\\/\\\\/')
CAMERA_COMPILE_HOST = $(shell uname -n)

$(CAMERA_GEN_DIR)/cam_generated_h: $(shell find . -iname "*.c") $(shell find . -iname "*.h") $(shell find . -iname "*.mk")
	mkdir -p $(CAMERA_GEN_DIR)
	echo '#define CAMERA_COMPILE_TIME "$(CAMERA_COMPILE_TIME)"' > $@
	echo '#define CAMERA_COMPILE_BY "$(CAMERA_COMPILE_BY)"' >> $@
	echo '#define CAMERA_COMPILE_HOST "$(CAMERA_COMPILE_HOST)"' >> $@

modules: $(CAMERA_GEN_DIR)/cam_generated_h

modules dtbs:
	$(MAKE) -C $(KERNEL_SRC) M=$(M) modules $(KBUILD_OPTIONS)

modules_install:
	$(MAKE) M=$(M) -C $(KERNEL_SRC) modules_install

clean:
	$(MAKE) -C $(KERNEL_SRC) M=$(M) clean
