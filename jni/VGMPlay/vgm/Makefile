########################
#
# VGMPlay Makefile
# (for GNU Make 3.81)
#
########################

# TODO: Make this look less horrible.

# Uncomment if you build on Windows using MinGW.
#WINDOWS = 1

# Uncomment if you want to use libao instead of OSS for sound streaming under Linux
#USE_LIBAO = 1

CC = gcc
PREFIX = /usr/local
MANPREFIX = $(PREFIX)/share/man

# -- VGMPlay specific Compile Flags --
MAINFLAGS := -DCONSOLE_MODE -DADDITIONAL_FORMATS -DSET_CONSOLE_TITLE
ifdef WINDOWS
# MinGW defines __WINDOWS__, Visual Studio defines WIN32
MAINFLAGS += -DWIN32
else
ifdef USE_LIBAO
MAINFLAGS += -DUSE_LIBAO
endif
endif
EMUFLAGS := -DENABLE_ALL_CORES

# -- General Compile Flags --
CFLAGS := -O3 -g0 $(CFLAGS)
# libm (math library) and libz (zlib)
LDFLAGS := -lm -lz $(LDFLAGS)
ifdef WINDOWS
# for Windows, add kernel32 and winmm (Multimedia APIs)
LDFLAGS += -lkernel32 -lwinmm
else
# for Linux, add librt (clock stuff) and libpthread (threads)
LDFLAGS += -lrt -lpthread -pthread
MAINFLAGS += -pthread
ifdef USE_LIBAO
LDFLAGS += -lao
endif
endif
# add Library Path, if defined
ifdef LD_LIBRARY_PATH
LDFLAGS += -L $(LD_LIBRARY_PATH)
endif

SRC = .
OBJ = obj
EMUSRC = $(SRC)/chips
EMUOBJ = $(OBJ)/chips

OBJDIRS = \
	$(OBJ) \
	$(EMUOBJ)
MAINOBJS = \
	$(OBJ)/VGMPlay.o \
	$(OBJ)/VGMPlay_AddFmts.o \
	$(OBJ)/VGMPlayUI.o \
	$(OBJ)/Stream.o \
	$(OBJ)/ChipMapper.o
ifdef WINDOWS
MAINOBJS += $(OBJ)/pt_ioctl.o
endif
EMUOBJS = \
	$(EMUOBJ)/262intf.o \
	$(EMUOBJ)/2151intf.o \
	$(EMUOBJ)/2203intf.o \
	$(EMUOBJ)/2413intf.o \
	$(EMUOBJ)/2608intf.o \
	$(EMUOBJ)/2610intf.o \
	$(EMUOBJ)/2612intf.o \
	$(EMUOBJ)/3526intf.o \
	$(EMUOBJ)/3812intf.o \
	$(EMUOBJ)/8950intf.o \
	$(EMUOBJ)/adlibemu_opl2.o \
	$(EMUOBJ)/adlibemu_opl3.o \
	$(EMUOBJ)/ay8910.o \
	$(EMUOBJ)/ay_intf.o \
	$(EMUOBJ)/c140.o \
	$(EMUOBJ)/c6280.o \
	$(EMUOBJ)/c6280intf.o \
	$(EMUOBJ)/dac_control.o \
	$(EMUOBJ)/emu2149.o \
	$(EMUOBJ)/emu2413.o \
	$(EMUOBJ)/fm2612.o \
	$(EMUOBJ)/fm.o \
	$(EMUOBJ)/fmopl.o \
	$(EMUOBJ)/gb.o \
	$(EMUOBJ)/k051649.o \
	$(EMUOBJ)/k053260.o \
	$(EMUOBJ)/k054539.o \
	$(EMUOBJ)/multipcm.o \
	$(EMUOBJ)/nes_apu.o \
	$(EMUOBJ)/nes_intf.o \
	$(EMUOBJ)/np_nes_apu.o \
	$(EMUOBJ)/np_nes_dmc.o \
	$(EMUOBJ)/np_nes_fds.o \
	$(EMUOBJ)/okim6258.o \
	$(EMUOBJ)/okim6295.o \
	$(EMUOBJ)/Ootake_PSG.o \
	$(EMUOBJ)/panning.o \
	$(EMUOBJ)/pokey.o \
	$(EMUOBJ)/pwm.o \
	$(EMUOBJ)/qsound.o \
	$(EMUOBJ)/rf5c68.o \
	$(EMUOBJ)/segapcm.o \
	$(EMUOBJ)/scd_pcm.o \
	$(EMUOBJ)/scsp.o \
	$(EMUOBJ)/scspdsp.o \
	$(EMUOBJ)/sn76489.o \
	$(EMUOBJ)/sn76496.o \
	$(EMUOBJ)/sn764intf.o \
	$(EMUOBJ)/upd7759.o \
	$(EMUOBJ)/ym2151.o \
	$(EMUOBJ)/ym2413.o \
	$(EMUOBJ)/ym2612.o \
	$(EMUOBJ)/ymdeltat.o \
	$(EMUOBJ)/ymf262.o \
	$(EMUOBJ)/ymf271.o \
	$(EMUOBJ)/ymf278b.o \
	$(EMUOBJ)/ymz280b.o \
	$(EMUOBJ)/ay8910_opl.o \
	$(EMUOBJ)/sn76496_opl.o \
	$(EMUOBJ)/ym2413hd.o \
	$(EMUOBJ)/ym2413_opl.o

vgmplay:	$(OBJDIRS) $(EMUOBJS) $(MAINOBJS)
	@echo Linking ...
	@$(CC) $(MAINOBJS) $(EMUOBJS) $(LDFLAGS) -o vgmplay
	@echo Done.

# compile the chip-emulator c-files
$(EMUOBJ)/%.o:	$(EMUSRC)/%.c
	@echo Compiling $< ...
	@$(CC) $(CFLAGS) $(EMUFLAGS) -c $< -o $@

# compile the main c-files
$(OBJ)/%.o:	$(SRC)/%.c
	@echo Compiling $< ...
	@$(CC) $(CFLAGS) $(MAINFLAGS) -c $< -o $@

$(OBJDIRS):
	mkdir $@

clean:
	@echo Deleting object files ...
	@rm -f $(MAINOBJS) $(EMUOBJS)
	@echo Done.

# Thanks to ZekeSulastin for the install and uninstall routines
install:	vgmplay
	install vgmplay -Dm755 $(DESTDIR)$(PREFIX)/bin/vgmplay
	install vgmplay.1 -Dm644 $(DESTDIR)$(MANPREFIX)/man1/vgmplay.1
	install VGMPlay.ini -Dm644 $(DESTDIR)$(PREFIX)/share/vgmplay/vgmplay.ini
	-install yrw801.rom -Dm644 $(DESTDIR)$(PREFIX)/share/vgmplay/yrw801.rom

# Install the "vgm-player" wrapper
play_inst:	install
	install vgm-player -Dm755 $(DESTDIR)$(PREFIX)/bin/vgm-player

uninstall:
	rm $(DESTDIR)$(PREFIX)/bin/vgmplay
	rm $(DESTDIR)$(PREFIX)/bin/vgm-player
	rm $(DESTDIR)$(MANPREFIX)/man1/vgmplay.1
	rm -rf $(DESTDIR)$(PREFIX)/share/vgmplay

.PHONY: clean install uninstall
