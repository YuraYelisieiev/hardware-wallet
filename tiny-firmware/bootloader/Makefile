NAME  = bootloader
BOOTLOADER = 1

#libskywallet.a
OBJS += startup.o
OBJS += buttons.o
OBJS += layout.o
OBJS += oled.o
OBJS += rng.o
OBJS += serialno.o
OBJS += setup.o
OBJS += supervise.o
OBJS += timer.o
OBJS += util.o
OBJS += memory.o
OBJS += ../gen/bitmaps.o
OBJS += ../gen/fonts.o
#end libskywallet.a

#skycoin-crypto
OBJS += $(TOP_DIR)vendor/skycoin-crypto/skycoin_signature.small.o
OBJS += $(TOP_DIR)vendor/skycoin-crypto/skycoin_crypto.small.o
OBJS += $(TOP_DIR)vendor/skycoin-crypto/tools/curves.small.o
OBJS += $(TOP_DIR)vendor/skycoin-crypto/tools/hmac.small.o
OBJS += $(TOP_DIR)vendor/skycoin-crypto/tools/secp256k1.small.o
OBJS += $(TOP_DIR)vendor/skycoin-crypto/tools/rand.o
OBJS += $(TOP_DIR)vendor/skycoin-crypto/tools/entropypool.o
# OBJS += $(TOP_DIR)vendor/skycoin-crypto/tools/bip32.o
#end skycoin-crypto

OBJS += bootloader.o
OBJS += signatures.o
OBJS += usb.o

OBJS += $(TOP_DIR)vendor/skycoin-crypto/tools/bignum.small.o
OBJS += $(TOP_DIR)vendor/skycoin-crypto/tools/ecdsa.small.o
OBJS += $(TOP_DIR)vendor/skycoin-crypto/tools/sha2.small.o
OBJS += $(TOP_DIR)vendor/skycoin-crypto/tools/memzero.small.o

SIGNATURE_PROTECT ?= 0
SIGNATURE_DEBUG ?= 0

CFLAGS += -DSIGNATURE_PROTECT=$(SIGNATURE_PROTECT)
CFLAGS += -DSIGNATURE_DEBUG=$(SIGNATURE_DEBUG)
CFLAGS += -DBOOTLOADER=1
CFLAGS += -DUSE_PRECOMPUTED_IV=0
CFLAGS += -DUSE_PRECOMPUTED_CP=0
#CFLAGS += -I$(MKFILE_DIR)

OPTFLAGS ?= -Os
DEBUG ?= 0

LDSCRIPT = bootloader-memory.ld
LDLIBS   += -lopencm3_stm32f2

pub_keys.h:
	./generate-pubkeys-cfile.py -pk $(FIRMWARE_SIGNATURE_PUB_KEY1) $(FIRMWARE_SIGNATURE_PUB_KEY2) $(FIRMWARE_SIGNATURE_PUB_KEY3) $(FIRMWARE_SIGNATURE_PUB_KEY4) $(FIRMWARE_SIGNATURE_PUB_KEY5)

include ../Makefile.include

align: $(NAME).bin
	echo "$(CFLAGS)"
	./firmware_align.py $(NAME).bin

%.o: ../%.c
	$(CC) $(CFLAGS) -MMD -MP -o $@ -c $<

%.o: ../%.s
	$(AS) $(CPUFLAGS) -o $@ $<

clean::
	rm -rf *.o
	rm -f pub_keys.h
