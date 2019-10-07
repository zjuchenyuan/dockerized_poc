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

RUN wget https://gitlab.gnome.org/GNOME/gdk-pixbuf/uploads/a68dee3aaf8b80634f0b10d3f536e714/poc1.zip &&\
    unzip poc1.zip &&\
    /gdk-pixbuf/_build/gdk-pixbuf/gdk-pixbuf-pixdata poc1 /dev/null || exit 0
