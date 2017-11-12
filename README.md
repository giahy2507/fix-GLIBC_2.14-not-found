# Fix "GLIBC_2.14 not found" when running Tensorflow on CentOS without sudo permission
This note aims to help you fix this bug.

## Install [Anaconda](https://www.anaconda.com)
Anaconda is a platform that you can easily install and manage Python packages in local directory (where we don't need "sudo" command to read or write).

In case of running Python program on a server without "sudo" permission, you can not install any python package via "sudo" command.

For example, "sudo pip install tensorflow". (pip and python are located in "/usr/bin/pip" and "/usr/bin/python")

That is a reason why Anaconda is the best choice in this situation.

In this tutorial, I will install [Anaconda2](https://www.anaconda.com/download/#linux) in home directory.

```bash
cd ~/
https://repo.continuum.io/archive/Anaconda2-5.0.1-Linux-x86_64.sh
sh Anaconda2-5.0.1-Linux-x86_64.sh
```

After this step, Anaconda2 is installed in your home directory "~/anaconda2"

## Install [Tensorflow](https://www.tensorflow.org/)
```bash
~/anaconda2/bin/pip install tensorflow==1.2.0
```

Try running Tensorflow program.
```bash
~/anaconda2/bin/python2
```

```python
>>> import tensorflow as tf
```
Although We have successfully install tensowflow in the first step, we may meet the following bug when import tensorflow.

```
ImportError: /lib64/libc.so.6: version `GLIBC_2.14' not found (required by /home/s1610434/anaconda2/lib/python2.7/site-packages/tensorflow/python/_pywrap_tensorflow_internal.so)
```

This bug caused by the old version of "**libc**" in the directory "/lib64" used by your CentOS server.

If you have sudo permission, you can update the "**libc**" to a new version. But we don't have.

To overcome this bug, I recommend using another **libc** which is downloaded by ourselves and located in the local directory.

## Download GLIBC_2.17

Although what we want is "GLIBC_2.14", but in my experience, after installing "GLIBC_2.14", our program then requires "GLIBC_2.17".

Please clone this project to get all installed files. It contains 4 files:

⋅⋅* glibc-2.17-55.el7_0.5.x86_64.rpm

⋅⋅* glibc-devel-2.17-55.el7_0.5.x86_64.rpm

⋅⋅* libstdc++-4.8.5-4.el7.x86_64.rpm

⋅⋅* libstdc++-devel-4.8.5-4.el7.x86_64.rpm

```bash
cd ~/
git clone https://github.com/giahy2507/fix-GLIBC_2.14-not-found
cd fix-GLIBC_2.14-not-found
```

Follow the following script to install "GLIBC_2.17"

```bash
mkdir libc2.17
cd libc2.17
rpm2cpio ../glibc-devel-2.17-55.el7_0.5.x86_64.rpm | cpio -id
rpm2cpio ../glibc-2.17-55.el7_0.5.x86_64.rpm | cpio -id

cd ../
mkdir libstdc++4.8.5
cd libstdc++4.8.5
rpm2cpio ../libstdc++-devel-4.8.5-4.el7.x86_64.rpm | cpio -id
rpm2cpio ../libstdc++-4.8.5-4.el7.x86_64.rpm | cpio -id
```

Before running Tensorflow program, let's add some environment variables in bash to make the command more clear.

```bash
setenv PATH ~/anaconda2/bin:${PATH}
setenv PYTHON_PATH ~/anaconda2
setenv LDPATH ~/fix-GLIBC_2.14-not-found/libc2.17/lib64/ld-linux-x86-64.so.2
setenv LDLIBS ~/fix-GLIBC_2.14-not-found/libstdc++4.8.5/usr/lib64:/usr/lib:/usr/lib64
setenv LDLIBS ~/fix-GLIBC_2.14-not-found/libc2.17/lib64:${LDLIBS}
```

Try running Tensorflow program again by the following command.

```bash
${LDPATH} --library-path ${LDLIBS} ${PYTHON_PATH}/bin/python
```

```python
>>> import tensorflow as tf
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> print sess.run(a+b)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'sess' is not defined
>>> sess = tf.Session()
2017-11-12 22:36:43.357338: W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE4.1 instructions, but these are available on your machine and could speed up CPU computations.
2017-11-12 22:36:43.357418: W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE4.2 instructions, but these are available on your machine and could speed up CPU computations.
2017-11-12 22:36:43.357444: W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use AVX instructions, but these are available on your machine and could speed up CPU computations.
>>> print sess.run(a+b)
42
>>>
```

This is my experience on fixing this bug for Anaconda2.

**However**, I have tried to fix this bug for Anaconda3, but I met an **error** "Segmentation fault (core dumped)" when I use the command "${LDPATH} --library-path ${LDLIBS} ${PYTHON_PATH}/bin/python"

If there are any mistakes, please don't hesitate to contact me to fix it.
