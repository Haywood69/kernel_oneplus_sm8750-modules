# SPDX-License-Identifier: GPL-2.0-only

VIDEO_ROOT := $(KERNEL_SRC)/$(M)
KBUILD_OPTIONS += VIDEO_ROOT=$(VIDEO_ROOT)
KBUILD_OPTIONS += KBUILD_EXTRA_SYMBOLS=$(M)/../mmrm-driver/Module.symvers

all: modules

# msm_video/driver and video/driver are tracked symlinks to driver/, so each
# module gets its own object directory without writing to the source tree.
modules:
	$(MAKE) -C $(KERNEL_SRC) M=$(M) modules $(KBUILD_OPTIONS)

modules_install:
	$(MAKE) INSTALL_MOD_STRIP=1 -C $(KERNEL_SRC) M=$(M) modules_install

%:
	$(MAKE) -C $(KERNEL_SRC) M=$(M) $@ $(KBUILD_OPTIONS)

clean:
	rm -f *.o *.ko *.mod.c *.mod.o *~ .*.cmd Module.symvers
	rm -rf .tmp_versions
