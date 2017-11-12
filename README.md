# fix GLIBC_2.14 not found when running Tensorflow on CentOS without sudo permission

# install anaconda
Anaconda is a platform that you can easily install Python packages and manage them in local directory (where we dont need "sudo" command to write files).
In case of runing Python program on server without "sudo" permission, you can not install any python package via sudo command.
For example, "sudo pip install tensorflow". (pip and python are located in "/usr/bin/pip" and "/usr/bin/python")
That is a reason why Anaconda is a best choose in this situation.
In this tutorial, I will install Anaconda3 corresponding to Python3.



