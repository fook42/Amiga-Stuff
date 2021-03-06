NAME := AmigaTestKit
VER := 1.12

OBJS += entry.o
OBJS += cancellation.o
OBJS += util.o
OBJS += build.o
OBJS += crash.o
OBJS += audio.o
OBJS += battclock.o
OBJS += cia.o
OBJS += floppy.o
OBJS += joymouse.o
OBJS += keyboard.o
OBJS += memory.o
OBJS += mfm.o
OBJS += serpar.o
OBJS += testkit.o
OBJS += video.o
OBJS += ptplayer/ptplayer.o

include ../base/Rules.mk

# build.o must be refreshed on every build
.PHONY: build.c

all: $(NAME)-v$(VER).zip

build.o: CFLAGS += -DVER="\"$(VER)\""

entry.o: AFLAGS += -DDETECT_MEMORY

$(NAME)-v$(VER).zip: $(NAME).adf $(NAME) $(NAME).info README.md $(NAME).elf
	$(RM) -r $(NAME)-v$(VER)
	mkdir -p $(NAME)-v$(VER)/debug
	cp $^ $(NAME)-v$(VER)
	mv $(NAME)-v$(VER)/$(NAME).elf $(NAME)-v$(VER)/debug
	$(RM) $@
	zip -r $@ $(NAME)-v$(VER)

$(NAME): $(NAME).exe
	cp $< $@
	chmod 0755 $@

$(NAME).exe: $(NAME).bin
	$(PYTHON) ../base/mk_amiga_exe.py $< $@
	$(PYTHON) ../inflate/pack_exe.py $@ $@.tmp
	mv $@.tmp $@

$(NAME).adf: bootblock.bin $(NAME).bin
	$(GZIP) $(NAME).bin
	../inflate/degzip -H $(NAME).bin.gz $(NAME).bin.tmp
	$(PYTHON) ../base/mk_adf.py bootblock.bin $(NAME).bin.tmp $@
	$(RM) $(NAME).bin.tmp

clean::
	$(RM) *.adf *.exe *.zip $(NAME) *.tmp *.gz
	$(RM) -r $(NAME)-v* ptplayer/*.o
