# Fix GLIBC_2.14 not found when running Tensorflow on CentOS without sudo permission
This note aims to help you fix this bug, ahihi.

## Install [Anaconda](https://www.anaconda.com)
Anaconda is a platform that you can easily install and manage Python packages in local directory (where we dont need "sudo" command to acess or write files).

In case of runing Python program on server without "sudo" permission, you can not install any python package via sudo command.

For example, "sudo pip install tensorflow". (pip and python are located in "/usr/bin/pip" and "/usr/bin/python")

That is a reason why Anaconda is a best choose in this situation.

In this tutorial, I will install [Anaconda3](https://www.anaconda.com/download/#linux) in home directory.

```bash
cd ~/
wget https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh
sh Anaconda3-5.0.1-Linux-x86_64.sh
```

After this step, Anaconda3 is install in your home directory "~/anaconda3"

## Install [Tensorflow](https://www.tensorflow.org/)
```bash
~/anaconda3/bin/pip install tensorflow==1.2.0
```

Try importing tensorflow in python program
```bash
~/anaconda3/bin/python3
```

```python
>>> import tensorflow as tf
```
Although We have sucessfully install tensowflow in the first step, we may meet the following bug when import tensorflow.

```
ImportError: /lib64/libc.so.6: version `GLIBC_2.14' not found (required by /home/s1610434/anaconda3/lib/python3.6/site-packages/tensorflow/python/_pywrap_tensorflow_internal.so)
```

This bug cause by the old version of "**libc**" in the directory "/lib64" used by your CentOS server.

If you have sudo permission, you can update the "**libc**" to a new version. But we don't have.

To overcome this bug, I recommend use another **libc** which is downloaded by ourselve and located in local directory.

## Download GLIBC_2.17

Although what we want is "GLIBC_2.14", but in my experience, after installing "GLIBC_2.14", our program then requires "GLIBC_2.17".

Please download all files in this project. It contain 4 files:

..* glibc-2.17-55.el7_0.5.x86_64.rpm

..* glibc-devel-2.17-55.el7_0.5.x86_64.rpm

..* libstdc++-4.8.5-4.el7.x86_64.rpm

..* libstdc++-devel-4.8.5-4.el7.x86_64.rpm



