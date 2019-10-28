FROM zjuchenyuan/afl
ENV AFL_USE_ASAN=1
RUN wget http://www.sentex.net/~mwandel/jhead/jhead-3.03.tar.gz &&\
    tar xf jhead-3.03.tar.gz &&\
    cd jhead-3.03 &&\
    make -j && mkdir /tmp/asan && cp jhead /tmp/asan/ &&\
    unset AFL_USE_ASAN && make clean && make -j && mkdir /tmp/justafl && cp jhead /tmp/justafl/

RUN git clone https://github.com/zjuchenyuan/fuzzpoc &&\
    apt install -y gdb valgrind

RUN /tmp/asan/jhead /fuzzpoc/jhead_poc1 || exit 0
RUN valgrind -v /tmp/justafl/jhead /fuzzpoc/jhead_poc1 || exit 0
RUN /tmp/asan/jhead /fuzzpoc/jhead_poc2 || exit 0
RUN valgrind -v /tmp/justafl/jhead /fuzzpoc/jhead_poc2 || exit 0
RUN /tmp/asan/jhead /fuzzpoc/jhead_poc3 || exit 0
RUN valgrind -v /tmp/justafl/jhead /fuzzpoc/jhead_poc3 || exit 0