FROM zjuchenyuan/afl

RUN apt update &&\
    apt install -y ninja-build libglib2.0-dev libpng12-dev libtiff5-dev gettext libgettextpo-dev &&\
    pip3 install meson &&\
    git clone https://gitlab.gnome.org/GNOME/gdk-pixbuf &&\
    cd gdk-pixbuf &&\
    git checkout 3c7740498fd31b6746dd7e04601886766a6644b7 &&\
    sed -i "1i#include <string.h>"  gdk-pixbuf/io-gif-animation.c &&\
    sed -i "57d" tests/meson.build &&\
    meson _build . -Dx11=false -Dman=false -Dinstalled_tests=false -Dgir=false -Dbuiltin_loaders=all -Ddefault_library=static &&\
    cd _build &&\
    AFL_USE_ASAN=1 ASAN_OPTIONS="detect_leaks=0" ninja

ADD . /

RUN ./gdk-pixbuf/_build/gdk-pixbuf/gdk-pixbuf-pixdata poc1_memset-negative-size-param_gdk_pixbuf_gif_anim_iter_get_pixbuf /dev/null || exit 0
RUN ./gdk-pixbuf/_build/gdk-pixbuf/gdk-pixbuf-pixdata poc2_heap-use-after-free_OneLine8 /dev/null || exit 0
RUN ./gdk-pixbuf/_build/gdk-pixbuf/gdk-pixbuf-pixdata poc3_heap-buffer-overflow_OneLine8 /dev/null || exit 0
RUN ./gdk-pixbuf/_build/gdk-pixbuf/gdk-pixbuf-pixdata poc4_heap-buffer-overflow_OneLine4 /dev/null || exit 0
