linux 下安装emacs24
Linux版本：gcc version 4.8.4 (Ubuntu 4.8.4-2ubuntu1~14.04.3) 
添加 PPA 到 apt repository 中:
$ sudo add-apt-repository ppa:cassou/emacs
$ sudo apt-get update
然后，你就可以使用apt-get方便地下载Emacs了（这个Emacs-snapshot是最新版本，平均两周更新一次）:
$ sudo apt-get install emacs-snapshot-el emacs-snapshot-gtk emacs-snapshot
 
如果偏好稳定版本，请用这个:
$ sudo apt-get install emacs24 emacs24-el emacs24-common-non-dfsg