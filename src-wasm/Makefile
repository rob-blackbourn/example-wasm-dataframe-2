SYSROOT=/home/rtb/dev/public/wasi-libc/sysroot

CC=clang
LD=wasm-ld
#CFLAGS=--target=wasm32-unknown-unknown-wasm --optimize=3 -nostdlib
CFLAGS=--target=wasm32-wasi-unknown-wasm --optimize=3 --sysroot=$(SYSROOT)
LDFLAGS=--export-all --no-entry --allow-undefined -L$(SYSROOT)/lib/wasm32-wasi -lc -lm

WASM2WAT=/opt/wabt/bin/wasm2wat

# sources = array-methods.c memory-allocation.c foo.c
sources = array-methods-int.c array-methods-double.c
objects = $(sources:%.c=%.bc)
target = data-frame

.PHONY: all

all: $(target).wat

$(target).wat: $(target).wasm
	$(WASM2WAT) $(target).wasm -o $(target).wat

$(target).wasm: $(objects)
	$(LD) $(objects) -L$(SYSROOT)/lib/wasm32-wasi/crt1.o $(LDFLAGS) -o $(target).wasm
	
%.bc: %.c
	$(CC) -c -emit-llvm $(CFLAGS) $< -o $@

clean:
	rm -f $(target).wasm
	rm -f $(target).wat
	rm -f $(objects)
