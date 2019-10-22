FROM zjuchenyuan/afl
ENV AFL_USE_ASAN=1
RUN git clone https://git.code.sf.net/p/mp3gain/code mp3gain-code &&\
    cd mp3gain-code/mp3gain &&\
    git checkout fc72045 &&\
    make -j && mkdir /tmp/asan && cp mp3gain /tmp/asan/ &&\
    unset AFL_USE_ASAN && make clean && make -j && mkdir /tmp/justafl && cp mp3gain /tmp/justafl/

RUN git clone https://github.com/zjuchenyuan/fuzzpoc &&\
    apt install -y gdb valgrind

RUN /tmp/asan/mp3gain /fuzzpoc/mp3gain_poc1 || exit 0
RUN valgrind -v /tmp/justafl/mp3gain /fuzzpoc/mp3gain_poc1 || exit 0
RUN /tmp/asan/mp3gain /fuzzpoc/mp3gain_poc2 || exit 0
RUN valgrind -v /tmp/justafl/mp3gain /fuzzpoc/mp3gain_poc2 || exit 0