# This makefile can help you to build test inputs for your overflower tool
# and check its results. If you wish to compute results using your tool as
# well, you must define the OVERFLOWER variable below to contain the path
# to your compiled overflower binary. If you wish to compile your own tests,
# set the paths for CLANG and OPT via the LLVM_PATH, as well.
#
# To build LLVM assembly files from C source files:
#   make llvmasm
#
# To analyze the inputs using your tool:
#   make analyze
#
# To remove previous output & intermediate files:
#   make clean
#

OVERFLOWER   := ../../overflowerbuild/bin/overflower
LLVM_PATH    :=
CLANG        := $(LLVM_PATH)clang
OPT          := $(LLVM_PATH)opt
RM           := /bin/rm
SOURCE_FILES := $(sort $(wildcard c/*.c))
ASM_FILES    := $(addprefix ll/,$(notdir $(SOURCE_FILES:.c=.ll)))
CSV_FILES    := $(addprefix csv/,$(notdir $(ASM_FILES:.ll=.csv)))


all: $(CSV_FILES)
llvmasm: $(ASM_FILES)
analyze: $(CSV_FILES)


ll/%.ll: c/%.c
	$(CLANG) -g -emit-llvm -O0 -S -Xclang -disable-O0-optnone $< -o - | $(OPT) -mem2reg -S -o $@

csv/%.csv: ll/%.ll
	$(OVERFLOWER) $< > $@

clean:
	$(RM) -f $(CSV_FILES)

veryclean: clean
	$(RM) -f $(ASM_FILES)

