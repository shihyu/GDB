###gdb-7.9 build  gdb server and gdb 

```
./configure --target=arm-linux --prefix=/usr/local/arm-gdb
# --target=arm-linux表示生成的gdb調試的目標是在arm核心Linux系統中運行的程序

make
sudo make install
```  

- Toolchain
- 無法使用 android 的 arm 編譯器目前原因不清楚
```
https://launchpad.net/linaro-toolchain-binaries/trunk/2013.10/+download/gcc-linaro-arm-linux-gnueabihf-4.8-2013.10_linux.tar.bz2
```

- build.env  
```
# -*- shell-script -*-
TOOLCHAIN=gcc-linaro-arm-linux-gnueabihf-4.8-2013.10_linux
DIR=$(pushd $(dirname $BASH_SOURCE) > /dev/null; pwd; popd > /dev/null)
echo $DIR
export PATH=${PATH}:${DIR}/${TOOLCHAIN}/bin
export CC=arm-linux-gnueabihf-gcc
```

```
source build.env
./configure --target=arm-linux --host=arm-linux LDFLAGS="-static"
# 這裡的--host指定這個程序的目標平台。這一步中會檢查系統中是否有交叉編譯器的。
# We must add the above LDFLAGS to let gdb statically linked, otherwise it cannot run on Android.

time make -j8 2>&1 | tee build.log
file ./gdbserver

./gdbserver: ELF 32-bit LSB  executable, ARM, EABI5 version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 3.1.1, BuildID[sha1]=4eec8a5a6893ed8ce65eea5d0741a55cc621236a, not stripped
```

- 要注意gdb和gdbserver版本一致
