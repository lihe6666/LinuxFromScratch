# Linux From Scratch

从零构建 Linux 系统。

[ Web Site ][1]

宿主主机：可以理解为重新构建linux系统的一个系统（环境），这里以ubuntu 18.04 为准。

虚拟主机：可以理解为当前主机（其实这里如果用实体机，完全可以省略这一步），这里以VirtualBox 为准。

安装系统不是问题，增加一个磁盘或者分区（虚拟机更简单）不是问题。那么问题来了。下载构建包是国外的网址，我下载了几个小时都没完成，后面使用了国外服务器才下载成功。

```bash
###安装开发环境
sudo apt install build-essential git texinfo bison
```

```bash
###检测当前系统环境
cat > version-check.sh << "EOF"
#!/bin/bash
# Simple script to list version numbers of critical development tools
export LC_ALL=C
bash --version | head -n1 | cut -d" " -f2-4
MYSH=$(readlink -f /bin/sh)
echo "/bin/sh -> $MYSH"
echo $MYSH | grep -q bash || echo "ERROR: /bin/sh does not point to bash"
unset MYSH

echo -n "Binutils: "; ld --version | head -n1 | cut -d" " -f3-
bison --version | head -n1

if [ -h /usr/bin/yacc ]; then
  echo "/usr/bin/yacc -> `readlink -f /usr/bin/yacc`";
elif [ -x /usr/bin/yacc ]; then
  echo yacc is `/usr/bin/yacc --version | head -n1`
else
  echo "yacc not found" 
fi

bzip2 --version 2>&1 < /dev/null | head -n1 | cut -d" " -f1,6-
echo -n "Coreutils: "; chown --version | head -n1 | cut -d")" -f2
diff --version | head -n1
find --version | head -n1
gawk --version | head -n1

if [ -h /usr/bin/awk ]; then
  echo "/usr/bin/awk -> `readlink -f /usr/bin/awk`";
elif [ -x /usr/bin/awk ]; then
  echo awk is `/usr/bin/awk --version | head -n1`
else 
  echo "awk not found" 
fi

gcc --version | head -n1
g++ --version | head -n1
ldd --version | head -n1 | cut -d" " -f2-  # glibc version
grep --version | head -n1
gzip --version | head -n1
cat /proc/version
m4 --version | head -n1
make --version | head -n1
patch --version | head -n1
echo Perl `perl -V:version`
python3 --version
sed --version | head -n1
tar --version | head -n1
makeinfo --version | head -n1  # texinfo version
xz --version | head -n1

echo 'int main(){}' > dummy.c && g++ -o dummy dummy.c
if [ -x dummy ]
  then echo "g++ compilation OK";
  else echo "g++ compilation failed"; fi
rm -f dummy.c dummy
EOF
```

[Build a basic Linux System from package.][2]

[1]:	http://www.linuxfromscratch.org/ "Linux From Scratch Web Site."
[2]:	http://www.linuxfromscratch.org/lfs/view/stable/wget-list "软件包"