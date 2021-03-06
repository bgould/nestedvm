Updated NestedVM toolchain
==========================

This repository contains an updated [NestedVM][1] toolchain that is suitable
for building modern C/C++ applications to pure Java bytecode.  The official
NestedVM project hosted at [nestedvm.ibex.org][1] only supports an old version
of GCC (3.3.6).  Fortunately, [Henry Wertz][2] has made available to use GCC 
4.8 and an updated newlib, and this repository includes those patches.

This repo also has some bug fixes by [David Ehrmann][3] to NestedVM's 
`UnixRuntime` and the parsing of ELF files generated by GCC 4.8.

Also incorporated in this repo are [John Stroy's][4] updates to
the NestedVM runtime.

## Installation

The NestedVM toolchain consists mainly of the following components:

 * Lightly patched GCC+binutils+newlib toolchain for compiling C/C++ programs
   to MIPS binaries
 * A compiler written in Java that is capable of translating MIPS binaries
   into both Java source code and Java bytecode (Java bytecode is the
   recommended target format)
 * A runtime support library that comes in 2 flavors, "standard" and "Unix".
   The standard runtime is a minimal runtime, just enough to support programs
   that only read and write from streams and memory.  The "Unix" runtime on the
   other hand provides a significant amount of functionality that should allow
   many posix programs to compile and run.

To build NestedVM, clone this repository and run `make dist`.  This might take
a while, but when finished you will have all of the above components available
in `upstream/install`.  The toolchain can be used from that location, or the
directory can be copied to some other location if you wish.  You can use
`make install` to achieve this (will install to `/usr/local/nestedvm` by
default).  If you want to install to some location other than the default,
specify the `prefix` environment variable.  For example, to install to 
`/tmp/nestedvm`, you can run `make install prefix=/tmp`.

*Known issue: the version of binutils being used does not build properly with
  GCC 4.9... please try GCC 4.8 or clang*

Below are some system-specific build instructions for this repository; pull
requests with steps for other environments are certainly welcome.

### Debian 8 etc:

    apt-get -y update && apt-get -y upgrade
    apt-get -y install build-essential libgmp-dev libmpc-dev libmpfr-dev git curl openjdk-7-jdk gcc-4.8 g++-4.8
    cd /usr/local/src
    git clone https://github.com/bgould/nestedvm
    cd /usr/local/src/nestedvm
    make dist CC=gcc-4.8 CXX=g++-4.8

### CentOS 7:

    yum -y update
    yum install -y java-1.7.0-openjdk-devel git gmp-devel libmpc-devel mpfr-devel
    yum -y groupinstall "Development Tools"
    cd /usr/local/src
    git clone https://github.com/bgould/nestedvm
    cd /usr/local/src/nestedvm
    make dist 

[1]: http://nestedvm.ibex.org/
[2]: http://article.gmane.org/gmane.comp.java.nestedvm/185
[3]: https://github.com/ehrmann/nestedvm
[4]: https://github.com/jdstroy/nestedvm
