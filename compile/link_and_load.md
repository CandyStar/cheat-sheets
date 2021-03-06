linking, loading, inspecting tools
====================

# Index

1. [nm](#nm)
1. [objdump](#objdump)
1. [ldd](#ldd)
1. [inspect memory mapping](#inspect memory mapping)
1. [readelf](#readelf)
1. [ld](#ld)
1. [reference](#reference)

<h2 id="nm">nm - list symbols from object files</h2>
```shell
gcc -c test.c -o test.o
nm test.o
```

`nm` could also list the symbols from **excutables and shared library**:
```shell
gcc test.o -o test

nm test

nm -D libxxx.so # list symbols in libxxx.so
```


<h2 id="objdump">objdump - display information from object files</h2>
```shell
gcc test.o -o test

objdump -D test | less
```


<h2 id="ldd">ldd - show shared library dependencies</h2>
`ldd` lists the shared library dependencies of an excutable file:
```shell
ldd a.out

linux-vdso.so.1 =>  (0x00007fff7fd9b000)
libc.so.6 => /lib64/libc.so.6 (0x0000003f42400000)
lib64/ld-linux-x86-64.so.2 (0x0000003f41c00000)
```

ldd 命令模拟加载可执行程序需要的动态链接库，但并不执行程序，后面的地址部分
表示模拟装载过程中动态链接库的地址。如果尝试多次运行 ldd 命令，
我们会发现每次动态链接库的地址都是不一样的，因为这个地址是动态定位的。
我们平常工作中，如果某一个二进制可执行文件报错找不到某个函数定义，
可以用这个命令检查是否系统丢失或者没有安装某一个动态链接库。



<h2 id="inspect memory mapping">inspect memory mapping</h2>
`cat /proc/<pid>/maps/` where <pid> is the process ID.

```shell
7feeef61f000-7feeef7d4000 r-xp 00000000 fd:01 135891 /lib/x86_64-linux-gnu/libc-2.15.so
7feeef7d4000-7feeef9d3000 ---p 001b5000 fd:01 135891 /lib/x86_64-linux-gnu/libc-2.15.so
7feeef9d3000-7feeef9d7000 r--p 001b4000 fd:01 135891 /lib/x86_64-linux-gnu/libc-2.15.so
7feeef9d7000-7feeef9d9000 rw-p 001b8000 fd:01 135891 /lib/x86_64-linux-gnu/libc-2.15.so
```


<h2 id="readelf">readelf - Displays information about ELF files</h2>
```shell
readelf -d a.out
```


<h2 id="ld">ld - loading tool</h2>
Generate a shared library with `ld`:
```shell
# alternative 1
gcc -fPIC -c -Wall test.c # PIC: Position Independent Code
ld -shared test.o -o libtest.so

# alternative 2
gcc -fPIC -shared -Wall -o libtest.so test.c

# generate shared lib, with specified version
gcc -fPIC -shared -Wall -Wl,-soname,libtest.so.0  -o libtest.so.0.0.0 test.c
```


# Reference
-------------
1. [高级语言的编译：链接及装载过程介绍](http://tech.meituan.com/linker.html)
1. [Linux动态库相关知识整理](http://zkt.name/linuxgong-xiang-ku-de-chuang-jian-yu-shi-yong/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
