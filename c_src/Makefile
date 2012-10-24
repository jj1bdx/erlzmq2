LINUX=$(shell uname | grep Linux | wc -l | xargs echo)
FREEBSD=$(shell uname | grep FreeBSD | wc -l | xargs echo)
DEPS=../deps

ifeq ($(LINUX),1)
PIC_FLAGS=--with-pic
endif

ifeq ($(FREEBSD),1)
PIC_FLAGS=--with-pic
endif

ZMQ_FLAGS=--without-documentation $(PIC_FLAGS)

ifndef ZEROMQ_VERSION
ZEROMQ_VERSION=master
endif

all: $(DEPS)/zeromq3/src/.libs/libzmq.a

clean:
	if test -e $(DEPS)/zeromq3/Makefile; then \
		cd $(DEPS)/zeromq3; gmake clean; \
	else \
		true; \
	fi

distclean:
	@rm -rf $(DEPS)

$(DEPS):
	@mkdir -p $(DEPS)

$(DEPS)/zeromq3: $(DEPS)
	@git clone https://github.com/zeromq/libzmq.git $(DEPS)/zeromq3
	@echo $(ZEROMQ_VERSION)
	@cd $(DEPS)/zeromq3 && git checkout $(ZEROMQ_VERSION)

$(DEPS)/zeromq3/src/.libs/libzmq.a: $(DEPS)/zeromq3
	@cd $(DEPS)/zeromq3 && ./autogen.sh && ./configure CC=gcc46 CXX=g++46 $(ZMQ_FLAGS) && gmake
