.PHONY: all reset

BINS=siolpain fmt stack cas hook hook2 stress1 fmtarr
BINS+=bench-expire expire dumptwice syncget nsabuse drain merge
BINS+=chkkey

CFLAGS+=-I../../src -I ../src ${USER_CFLAGS}
LDFLAGS+=../src/libsdb.a ${USER_LDFLAGS}
SCRIPTS=createfail.sh testgrep.sh delkey.sh
ALL=all

all: $(BINS) $(SCRIPTS) reset
	@suite/run.sh
	-cd api && $(MAKE)
	@$(MAKE) -C unit run

me:
	$(MAKE) -C ../src
	$(MAKE)

ASAN_CFLAGS=-fsanitize=address -fsanitize=undefined

asan:
	$(MAKE) CFLAGS="${ASAN_CFLAGS} ${CFLAGS}" ALL=asan all

$(SCRIPTS):
	./$@

$(BINS):
	$(CC) -o $@ $@.c $(LDFLAGS) $(CFLAGS)
	$(SHELL) -c "time ./$@"

b: $(BINS)

reset:
	cd reset && $(MAKE) $(ALL)

add10k:
	sh add10k.sh

clean:
	rm -f f dumptwice.db
	rm -f a test.db db a.gz a.out $(BINS)

.PHONY: ${SCRIPTS} ${BINS} b
