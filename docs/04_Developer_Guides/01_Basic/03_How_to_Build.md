---
title: How to build
---

## Setup Cross SDK environment

Source the SDK environment setup, each time you wish to use the SDK in a new shell session :

```sh
$ source ~/agl-app/agl-sdk/environment-setup-corei7-64-agl-linux
```


## Build C source file

For example, you have a C language source code that name is example.c.

#### example.c:
```C
#include <stdio.h>

int main(void)
{

	return 0;
}
```

#### Build:
```sh
$ $CC example.c -o example
```

You copy 'example' to the target root filesystem on the SD card.  The 'example' shall set the x attribute.

After the target device was booting,  you execute 'example' in the console.


## Build autoconf/automake 

For example, you have a autoconf/automake based source tree.

#### configure:

If you want to install default directory.

```sh
$ ./configure $CONFIGURE_FLAGS
```

If you want to install /usr directory.

```sh
$ ./configure $CONFIGURE_FLAGS --prefix=/usr
```

#### make:

```sh
$ make
```

#### install:

```sh
$ make install DESTDIR=/path/to/install/top/dir
```








