avr gcc freestanding
=================

~~~~
BASE=$HOME/avr-gcc-freestanding
mkdir $BASE
cd $BASE

mkdir $BASE/build
PREFIX=$BASE/build
export PREFIX

PATH=$BASE/build/bin:$PATH
export PATH
~~~~

~~~~
wget https://ftp.gnu.org/gnu/binutils/binutils-2.41.tar.bz2
bunzip2 -c binutils-2.41.tar.bz2  | tar xf -
cd binutils-2.41
mkdir obj-avr
cd obj-avr/
../configure --prefix=$PREFIX  --target=avr --disable-nls
make -j4
make install
~~~~

~~~~
wget https://ftp.gnu.org/gnu/gcc/gcc-13.2.0/gcc-13.2.0.tar.gz
gunzip -c gcc-13.2.0.tar.gz | tar xf -
cd gcc-13.2.0/
mkdir obj-avr
cd obj-avr/
../configure --prefix=$PREFIX --target=avr --enable-languages=c,c++ --disable-nls --disable-libssp --with-dwarf2
make -j4
make install
~~~~

~~~~
wget http://download.savannah.gnu.org/releases/avr-libc/avr-libc-2.1.0.tar.bz2
bunzip2 -c avr-libc-2.1.0.tar.bz2  | tar xf -
cd avr-libc-2.1.0/
mkdir obj-avr
cd obj-avr
../bootstrap
../configure --prefix=$PREFIX --build=`../config.guess` --host=avr
make -j4
make install
~~~~

~~~~
cd gcc-13.2.0/avr-obj
../configure --prefix=$PREFIX --target=avr --enable-languages=c,c++ --disable-nls --disable-libssp --with-dwarf2 --with-newlib --disable-__cxa_atexit --disable-threads --disable-shared --disable-sjlj-exceptions --enable-libstdcxx --disable-hosted-libstdcxx --disable-bootstrap
make -j4
make install
~~~~

Result
======
Built on Linux (Manjaro) and Windows (msys2).

~~~~~
$ ls $PREFIX/bin/
avr-addr2line.exe  avr-as.exe   avr-c++filt.exe  avr-elfedit.exe  avr-gcc.exe         avr-gcc-ar.exe  avr-gcc-ranlib.exe  avr-gcov-dump.exe  avr-gprof.exe   avr-ld.exe        avr-man     avr-objcopy.exe  avr-ranlib.exe   avr-size.exe     avr-strip.exe
avr-ar.exe         avr-c++.exe  avr-cpp.exe      avr-g++.exe      avr-gcc-13.2.0.exe  avr-gcc-nm.exe  avr-gcov.exe        avr-gcov-tool.exe  avr-ld.bfd.exe  avr-lto-dump.exe  avr-nm.exe  avr-objdump.exe  avr-readelf.exe  avr-strings.exe

$ ls $PREFIX/avr/include/c++/13.2.0/
algorithm  atomic  backward  bits    cfloat   compare   coroutine  cstdint  cxxabi.h  exception  ext         initializer_list  limits  new      numeric   ranges  scoped_allocator  span         tuple        typeindex  utility  version
array      avr     bit       bitset  climits  concepts  cstddef    cstdlib  debug     expected   functional  iterator          memory  numbers  optional  ratio   source_location   string_view  type_traits  typeinfo   variant
~~~~~

Sources
=======
* https://www.nongnu.org/avr-libc/user-manual/install_tools.html
* https://gist.github.com/ricardocosme/5ec8ad05b5f4adb66464a146dcc41545
* https://github.com/ZakKemble/avr-gcc-build/blob/master/avr-gcc-build.sh

