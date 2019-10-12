FROM zjuchenyuan/afl

ENV AFL_USE_ASAN=1 ASAN_OPTIONS="detect_leaks=0"
RUN git clone https://github.com/ThomasDickey/ncurses-snapshots &&\
    cd ncurses-snapshots &&\
    git checkout 8805e9e &&\
    ./configure --disable-shared &&\
    make -j &&\
    cp ./progs/tic /tmp/infotocap

RUN git clone https://github.com/zjuchenyuan/fuzzpoc
RUN /tmp/infotocap fuzzpoc/infotocap_poc1 || exit 0
RUN /tmp/infotocap fuzzpoc/infotocap_poc2 || exit 0
RUN /tmp/infotocap fuzzpoc/infotocap_poc3 || exit 0
RUN /tmp/infotocap fuzzpoc/infotocap_poc4 || exit 0
RUN /tmp/infotocap fuzzpoc/infotocap_poc5 || exit 0
RUN /tmp/infotocap fuzzpoc/infotocap_poc6 || exit 0
RUN /tmp/infotocap fuzzpoc/infotocap_poc7 || exit 0

RUN apt install -y valgrind &&\
    cd ncurses-snapshots &&\
    make clean &&\
    unset AFL_USE_ASAN &&\
    make -j &&\
    mkdir /tmp/noasan &&\
    cp ./progs/tic /tmp/noasan/infotocap

RUN valgrind -v /tmp/noasan/infotocap fuzzpoc/infotocap_poc1 || exit 0
RUN valgrind -v /tmp/noasan/infotocap fuzzpoc/infotocap_poc2 || exit 0
RUN valgrind -v /tmp/noasan/infotocap fuzzpoc/infotocap_poc3 || exit 0
RUN valgrind -v /tmp/noasan/infotocap fuzzpoc/infotocap_poc4 || exit 0
RUN valgrind -v /tmp/noasan/infotocap fuzzpoc/infotocap_poc5 || exit 0
RUN valgrind -v /tmp/noasan/infotocap fuzzpoc/infotocap_poc6 || exit 0
RUN valgrind -v /tmp/noasan/infotocap fuzzpoc/infotocap_poc7 || exit 0
