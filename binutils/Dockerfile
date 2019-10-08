FROM zjuchenyuan/afl
ENV AFL_USE_ASAN=1
RUN git clone git://sourceware.org/git/binutils-gdb.git --depth 50 &&\
    cd binutils-gdb &&\
    git checkout 816228ed09dc867fa16dc5458277d649885d98fe &&\
    ./configure --disable-shared &&\
    for i in bfd libiberty opcodes libctf; do cd $i; ./configure --disable-shared && make -j; cd ..; done  &&\
    cd binutils  &&\
    ./configure --disable-shared &&\
    make objdump nm-new size readelf cxxfilt

RUN apt install -y gdb &&\
    echo -e "set pagination off\nset confirm off" > /root/.gdbinit

ADD . /
RUN ./binutils-gdb/binutils/nm-new -A -a -l -S -s --special-syms --synthetic --with-symbol-versions -D poc1_segv__bfd_dwarf2_find_nearest_line || exit 0
RUN ./binutils-gdb/binutils/nm-new -A -a -l -S -s --special-syms --synthetic --with-symbol-versions -D poc2_excessive-malloc__bfd_x86_elf_get_synthetic_symtab || exit 0

# we may need to compile again without ASAN to use gdb

RUN gdb ./binutils-gdb/binutils/nm-new -ex 'r -A -a -l -S -s --special-syms --synthetic --with-symbol-versions -D poc3_malloc-memory-corruption_print_symbol' -ex bt -ex quit
RUN gdb ./binutils-gdb/binutils/nm-new -ex 'r -A -a -l -S -s --special-syms --synthetic --with-symbol-versions -D poc4_invalid-free__bfd_dwarf2_cleanup_debug_info' -ex bt -ex quit
RUN ./binutils-gdb/binutils/nm-new -A -a -l -S -s --special-syms --synthetic --with-symbol-versions -D poc5_invalid-free__bfd_dwarf2_cleanup_debug_info || exit 0
RUN ./binutils-gdb/binutils/nm-new -A -a -l -S -s --special-syms --synthetic --with-symbol-versions -D poc6_stack-overflow_find_abstract_instance || exit 0