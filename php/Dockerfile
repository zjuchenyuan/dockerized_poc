FROM zjuchenyuan/afl

RUN apt update && apt install -y libxml2-dev libsqlite3-0 libsqlite3-dev &&\
    wget https://www.php.net/distributions/php-7.4.4.tar.gz &&\
    tar xf php-7.4.4.tar.gz &&\
    cd php-7.4.4 &&\
    ./configure --disable-shared &&\
    make -j4 &&\
    cp sapi/cli/php /tmp/php_gcc &&\
    make clean &&\
    AFL_USE_ASAN=1 make -j4 &&\
    cp sapi/cli/php /tmp/php_asan &&\
    make clean

ADD . /data