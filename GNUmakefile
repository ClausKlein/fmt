# Standard stuff

.SUFFIXES:

MAKEFLAGS+= --no-builtin-rules  # Disable the built-in implicit rules.
MAKEFLAGS+= --warn-undefined-variables        # Warn when an undefined variable is referenced.

export CC:=gcc-16
export CXX:=g++-16
export CXXFLAGS:=-stdlib=libstdc++

.PHONY: all test check clean

all: build
	ninja -C build all # TODO: all_verify_interface_header_sets

build: GNUmakefile CMakeLists.txt
	cmake --version
	cmake -G Ninja -S . -B build --fresh \
	  -D FMT_MODULE=1 -D FMT_IMPORT_STD=0 -D CMAKE_CXX_STANDARD=23 \
	  -D CMAKE_EXPERIMENTAL_CXX_IMPORT_STD=f35a9ac6-8463-4d38-8eec-5d6008153e7d
	ln -fs build/compile_commands.json .

clean:
	rm -rf build .cache compile_commands.json
	find . -name .DS_Store -delete
	find . -name '*~' -delete

check: build
	run-clang-tidy src

test: build
	ninja -C build $(@)

# Anything we don't know how to build will use this rule.
% ::
	ninja -C build $(@)
