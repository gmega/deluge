# Building OpenSSL for Windows

These steps were sourced from http://www.magicsplat.com/blog/building-openssl-tls/

## Install Dependencies

* [ActivePerl](http://www.activestate.com/activeperl/downloads)
* [nasm](http://www.nasm.us/pub/nasm/releasebuilds/?C=M;O=D)
* [MS VC++ Compiler for Python](http://www.microsoft.com/en-us/download/details.aspx?id=44266)

## Build

* Extract OpenSSL source and navigate to it in VC++ prompt.

* In Visual C++ 2008 32-bit Command Prompt:

 ```
 set PATH=C:\Users\IEUser\AppData\Local\nasm;%PATH%
 perl Configure VC-WIN32 --prefix=C:\OpenSSL-Win32
 ms\do_nasm
 nmake -f ms\ntdll.mak
 nmake -f ms\ntdll.mak test
 nmake -f ms\ntdll.mak install
```

## Precompiled
There are precompiled but need to ensure the compiler is `msvc 9.0 (2008)` along with the openssl `include` and `lib` folders for libtorrent to link to.

* http://www.npcglib.org/~stathis/blog/precompiled-openssl/
* https://indy.fulgan.com/SSL/?C=M;O=A