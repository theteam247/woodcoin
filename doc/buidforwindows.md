# Cross-Compiling the exe on ubuntu.

##### 1.install cross-compile tools

```
sudo apt-get install software-properties-common lsb-release
sudo apt-get install binutils-mingw-w64-i686
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 86B72ED9
sudo add-apt-repository "deb [arch=amd64] https://pkg.mxe.cc/repos/apt `lsb_release -sc` main"
sudo apt-get update
sudo apt-get install mxe-i686-w64-mingw32.static-cc
export PATH=/usr/lib/mxe/usr/bin:$PATH
sudo sed -i 's/secure_path="/secure_path="\/usr\/lib\/mxe\/usr\/bin:/g' /etc/sudoers
```

##### 2.Build boost for windows

```
$ wget https://downloads.sourceforge.net/project/boost/boost/1.67.0/boost_1_67_0.tar.bz2
$ tar -xjvf boost_1_67_0.tar.bz2
$ cd boost_1_67_0
$ ./bootstrap.sh --without-icu
$ echo "using gcc : mxe : i686-w64-mingw32.static-g++ : <rc>i686-w64-mingw32.static-windres <archiver>i686-w64-mingw32.static-ar <ranlib>i686-w64-mingw32.static-ranlib ;" > user-config.jam
$ sudo ./b2 toolset=gcc address-model=32 target-os=windows variant=release threading=multi threadapi=win32 \
	link=static runtime-link=static --prefix=/usr/lib/mxe/usr/i686-w64-mingw32.static --user-config=user-config.jam \
	--without-mpi --without-python -sNO_BZIP2=1 --layout=tagged install
$ cd ..	
```

##### 3.build openssl for windows

```
wget https://www.openssl.org/source/old/1.1.0/openssl-1.1.0g.tar.gz
tar -xzvf openssl-1.1.0g.tar.gz
cd openssl-1.1.0g
CROSS_COMPILE="i686-w64-mingw32.static-" ./Configure mingw no-asm no-shared --prefix=/usr/lib/mxe/usr/i686-w64-mingw32.static
sudo make
sudo make install
cd ..
```

##### 4.Compiling berkley db: Download and unpack berkeley db:

```
wget http://download.oracle.com/berkeley-db/db-5.3.28.tar.gz
tar zxvf db-6.2.32.tar.gz
cd db-6.2.32
sed -i "s/WinIoCtl.h/winioctl.h/g" src/dbinc/win_db.h
export CC=/usr/lib/mxe/usr/bin/i686-w64-mingw32.static-gcc
export CXX=/usr/lib/mxe/usr/bin/i686-w64-mingw32.static-g++
./dist/configure \
	--disable-replication \
	--enable-mingw \
	--enable-cxx \
	--host x86 \
	--prefix=/usr/lib/mxe/usr/i686-w64-mingw32.static
make
sudo make install
cd ..


```

##### 5.Compiling miniupnpc: Download and unpack miniupnpc:

```
wget http://miniupnp.free.fr/files/miniupnpc-1.9.tar.gz
tar zxvf miniupnpc-1.9.tar.gz
cd miniupnpc-1.9
touch compile-m.sh
chmod ugo+x compile-m.sh
echo '#!/bin/bash 
MXE_PATH=/mnt/mxe
CC=$MXE_PATH/usr/bin/i686-w64-mingw32.static-gcc \
AR=$MXE_PATH/usr/bin/i686-w64-mingw32.static-ar \
CFLAGS="-DSTATICLIB -I$MXE_PATH/usr/i686-w64-mingw32.static/include" \
LDFLAGS="-L$MXE_PATH/usr/i686-w64-mingw32.static/lib" \
make libminiupnpc.a
mkdir $MXE_PATH/usr/i686-w64-mingw32.static/include/miniupnpc
cp *.h $MXE_PATH/usr/i686-w64-mingw32.static/include/miniupnpc
cp libminiupnpc.a $MXE_PATH/usr/i686-w64-mingw32.static/lib'>compile-m.sh
sudo ./compile-m.sh
cd..

```

##### 6.Build qrencode:

```
git clone https://github.com/ciphrex/mSIGNA.git
cd mSIGNA/deps/qrencode-3.4.3
./configure --host=i686-w64-mingw32.static --prefix=/usr/lib/mxe/usr/i686-w64-mingw32.static --without-tools --enable-static --disable-shared
```

##### 7.build exe

```
make -f makefile.linux-mingw
```

