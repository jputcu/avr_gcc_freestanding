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

Sources
=======
* https://www.nongnu.org/avr-libc/user-manual/install_tools.html
* https://gist.github.com/ricardocosme/5ec8ad05b5f4adb66464a146dcc41545
* https://github.com/ZakKemble/avr-gcc-build/blob/master/avr-gcc-build.sh

