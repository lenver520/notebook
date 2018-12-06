# TensorFlow And Python

## 安装

1. 安装 [Anaconda3](https://www.anaconda.com)
2. python环境设置

```bash
pip install PyHamcrest
python -m pip install --upgrade pip
pip install flake8
pip install yapf
conda search --full-name python
```

3. 安装TensorFlow环境

```bash
conda create -n tensorflow pip python=3.5
activate tensorflow
# 安装CPU版本
pip install –-ignore-installed –-upgrade tensorflow
# 安装GPU版本
pip install --ignore-installed --upgrade tensorflow-gpu
```

4. 安装常用Python库

```bash
pip install numpy
pip install matplotlib
```

5. TensorFlow测试

```py
import os
import tensorflow as tf
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'

hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))

a = tf.constant(10)
b = tf.constant(32)
print(sess.run(a+b))
```

## 交叉编译

### CMake交叉编译设置文件`toolChain.cmake`

```cmake
# this is required
SET(CMAKE_SYSTEM_NAME Linux)

# specify the cross compiler
SET(CMAKE_C_COMPILER   ${CC})
SET(CMAKE_CXX_COMPILER ${CXX})

# where is the target environment 
# SET(CMAKE_FIND_ROOT_PATH  /opt/arm/ppc_74xx /home/rickk/arm_inst)

# search for programs in the build host directories (not necessary)
SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
# for libraries and headers in the target directories
SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

# configure Boost and Qt
# SET(QT_QMAKE_EXECUTABLE /opt/qt-embedded/qmake)
# SET(BOOST_ROOT /opt/boost_arm)

```

### Gflags

    git clone https://github.com/gflags/gflags.git
    cd gflags
    mkdir build
    cd build
    cmake .. -DCMAKE_TOOLCHAIN_FILE=../../toolChain.cmake -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_SHARED_LIBS=1
    make -j4
    make install DESTDIR=../../install

### Glog

    git clone https://github.com/google/glog.git
    cd glog
    mkdir build
    cd build
    cmake .. -DCMAKE_TOOLCHAIN_FILE=../../toolChain.cmake -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_SHARED_LIBS=1
    make -j4
    make install DESTDIR=../../install

### Snappy

    git clone https://github.com/google/snappy.git
    cd snappy
    mkdir build
    cmake .. -DCMAKE_TOOLCHAIN_FILE=../../toolChain.cmake -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_SHARED_LIBS=1
    make -j4
    make install DESTDIR=../../install

### Protobuf

    git clone https://github.com/protocolbuffers/protobuf.git
    cd protobuf

- 编译Host版本

```shell
./autogen.sh
./configure --prefix=/usr
make -j4
sudo make install
```

- 编译Target版本

```shell
./configure --prefix=/usr --host=arm-poky-linux-gnueabi --target=arm-poky-linux-gnueabi --enable-static=0 --with-protoc=/usr/bin/protoc
make -j4
make install DESTDIR=../install
```

### OpenCV(V3.3.1)

- `3rdparty/libpng/pngpriv.h`文件128行修改如下：

```c
if defined(PNG_ARM_NEON) && (defined(__ARM_NEON__) || defined(__ARM_NEON)) && \
```

    cmake .. -DCMAKE_TOOLCHAIN_FILE=../../toolChain.cmake -DCMAKE_BUILD_TYPE="Release" -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_SHARED_LIBS=1 -DBUILD_opencv_apps=0 -DBUILD_TESTS=0 -DBUILD_PERF_TESTS=0 -DWITH_GSTREAMER=0 -DENABLE_PRECOMPILED_HEADERS=0
    make -j4
    make install DESTDIR=../../install

### OpenBLAS

    git clone https://github.com/xianyi/OpenBLAS.git
    cd OpenBLAS
    LC_ALL="C" make -j2 TARGET=ARMV7 CROSS=1 BINARY=32 NOFORTRAN=1 HOSTCC=gcc CROSS_SUFFIX=arm-poky-linux-gnueabi-
    make PREFIX=../install/usr install

### LMDB

    git clone https://github.com/LMDB/lmdb.git
    cd lmdb/libraries/liblmdb
    vi Makefile
    make -j4
    make install DESTDIR=../../../install

### leveldb

    git clone https://github.com/google/leveldb.git
    cd leveldb
    mkdir build
    cmake .. -DCMAKE_TOOLCHAIN_FILE=../../toolChain.cmake -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_SHARED_LIBS=1
    make -j4
    make install DESTDIR=../../install

### Boost(V1.68)

[参考](https://www.cnblogs.com/findumars/p/7461244.html)

    ./bootstrap.sh --prefix=/usr
    vi project-config.jam
    ./bjam --build-dir=obj variant=release link=shared threading=multi runtime-link=shared
    ./bjam install --prefix=../install/usr --build-dir=obj variant=release link=shared threading=multi runtime-link=shared

### HDF5(1.10)

    mkdir build
    cd build
    cmake .. -DCMAKE_TOOLCHAIN_FILE=../../toolChain.cmake -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_SHARED_LIBS=1 -DBUILD_TESTING=0 -DHDF5_BUILD_EXAMPLES=0

### Caffe

    git clone https://github.com/BVLC/caffe.git
    cd caffe
    mkdir build
    cd build
    cmake .. -DCMAKE_TOOLCHAIN_FILE=../../toolChain.cmake -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_SHARED_LIBS=1 -DCPU_ONLY=1 -DBUILD_docs=0 

