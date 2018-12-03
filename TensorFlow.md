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

4. TensorFlow测试

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

## 开发
