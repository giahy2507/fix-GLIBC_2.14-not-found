# Fix GLIBC_2.14 not found when running Tensorflow on CentOS without sudo permission
This note aims to help you fix this bug.

# Install [Anaconda](https://www.anaconda.com)
Anaconda is a platform that you can easily install Python packages and manage them in local directory (where we dont need "sudo" command to write files).

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

# Install [Tensorflow](https://www.tensorflow.org/)
```bash
~/anaconda3/bin/pip install tensorflow==1.2.0
```


