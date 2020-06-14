SYSROOT=/opt/wasi-libc

LLVM_VERSION=10
CC=clang-$(LLVM_VERSION)
LD=wasm-ld-$(LLVM_VERSION)
CFLAGS=--target=wasm32-unknown-unknown-wasm --optimize=3 --sysroot=$(SYSROOT)
LDFLAGS=--export-all --no-entry --allow-undefined -L$(SYSROOT)/lib/wasm32-wasi -lc -lm

WASM2WAT=/opt/wabt/bin/wasm2wat

sources = array-methods-int.c array-methods-double.c
objects = $(sources:%.c=%.bc)
target = data-frame

.PHONY: all

all: $(target).wat

$(target).wat: $(target).wasm
	$(WASM2WAT) $(target).wasm -o $(target).wat

$(target).wasm: $(objects)
	$(LD) $(objects) $(LDFLAGS) -o $(target).wasm
	
%.bc: %.c
	$(CC) -c -emit-llvm $(CFLAGS) $< -o $@

clean:
	rm -f $(target).wasm
	rm -f $(target).wat
	rm -f $(objects)
