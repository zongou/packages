# code-server-termux
    this repository is for making code-server deb package for termux.


## step1, compile code-server

### change repo and update if necessarily
    $ termux-change-repo

### Install dependencies, including python, nodejs, and yarn:
    $ pkg install -y python nodejs yarn git
### use Alibaba agent to speed up downloading if in CN
    $ yarn config set registry https://registry.npm.taobao.org
### Install code-server, this step will take a while
    $ yarn global add code-server
### test code-server
    $ code-server -v

## step2, pack to deb file
### make a dir, let's assume it fakeroot
    $ mkdir ~/fakeroot

### copy and order the compiled program data into it
    $ cd ~/fakeroot
    $ mkdir -p data/data/com.termux/files/usr/bin/
    $ cp $PREFIX/bin/code-server /data/data/com.termux/files/usr/bin/
    $ mkdir -p data/data/com.termux/files/home/.config
    $ cp -r ~/.config/yarn/ data/data/com.termux/files/home/.config/

### create 'DEBIAN' folder and 'control', 'md5sums' files
# mkdir DEBIAN
### create a 'control' file or copy from elsewhere
    $ cp yourControlFile DEBIAN/control
### make a 'md5sum' file, this step is not necessarily
    $ md5sum $(find usr -type f) > DEBIAN/md5sums

### pack to deb file
    $ dpkg -b ~/fakeroot/ code-server.deb

### update code-server
    $ yarn global remove code-server
    $ yarn global add code-server

## note:
    code-server runs depending on nodejs, make sure it is installed  

    to fix keyBack issue, goto setting, search json(shows as the first),  
    edit setting.json(click Edit in setting json hyper link),   
    add 
        "keyboard.dispatch": "keyCode"  
    as root element   

    If code does not run, click Menu -- Run -- Open Configurations

### reference: 
  https://gist.github.com/ppoffice/b9e88c9fd1daf882bc0e7f31221dda01  
  https://dev.to/codeledger/how-to-get-visual-studio-code-to-run-in-termux-on-android-405j  
