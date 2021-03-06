CFLAGS += -I../..

LDFLAGS += -L../../framework -lmlt -lpthread

include ../../../config.mak

TARGET = ../libmltrtaudio$(LIBSUF)

OBJS = consumer_rtaudio.o \
	RtAudio.o

SRCS := $(OBJS:.o=.cpp)

CXXFLAGS += $(CFLAGS) -Wno-deprecated -Wno-multichar -fno-rtti

ifeq ($(targetos), Darwin)
CXXFLAGS += -D__MACOSX_CORE__
LDFLAGS += -framework CoreAudio -framework CoreFoundation
else ifeq ($(targetos), MinGW)
CXXFLAGS += -D__WINDOWS_DS__
LDFLAGS += -lole32 -ldsound -lwinmm
# For ASIO when ready to try that:
#OBJS += asio.o asiodrivers.o asiolist.o iasiothiscallresolver.o
#CXXFLAGS +=-D__WINDOWS_ASIO__
else ifeq ($(targetos), Linux)
CXXFLAGS += -D__LINUX_ALSA__
CXXFLAGS += `pkg-config --cflags alsa`
LDFLAGS += `pkg-config --libs alsa`
else ifeq ($(targetos), NetBSD)
CXXFLAGS += -D__LINUX_OSS__
LDFLAGS += -lossaudio
else # FreeBSD
CXXFLAGS += -D__LINUX_OSS__
endif

all: 	$(TARGET)

$(TARGET): $(OBJS)
		$(CXX) $(SHFLAGS) -o $@ $(OBJS) $(LDFLAGS)

depend:	$(SRCS)
		$(CXX) -MM $(CFLAGS) $^ 1>.depend

distclean:	clean
		rm -f .depend

clean:
		rm -f $(OBJS) $(TARGET)

install: all
	install -m 755 $(TARGET) "$(DESTDIR)$(moduledir)"
	#install -d "$(DESTDIR)$(mltdatadir)/rtaudio"
	#install -m 644 *.yml "$(DESTDIR)$(mltdatadir)/rtaudio"

uninstall:
	rm "$(DESTDIR)$(moduledir)/libmltrtaudio$(LIBSUF)" 2> /dev/null || true
	rm -rf "$(DESTDIR)$(mltdatadir)/rtaudio"

ifneq ($(wildcard .depend),)
include .depend
endif
