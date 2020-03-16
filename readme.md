## Install dtrace from source on Mac OS Catalina 10.15.3

Make sure you have the xcode command line tools installed (im 90% sure this is needed, was busy trying to build the mac kernel).

```bash
xcode-select --install
```

## dtrace-tools

Best way is to try macports.
install macports
https://www.macports.org/install.php

Then run:
```bash
sudo port install dtrace
```

I tried macports and it was broken for catalina, got it working off git though.
I had to download some header files though:
- link.h
- msg.h
- dwarf.h
- elf.h
- gelf.h
- libdwarf.h
- libelf.h

I just added these to the include directory.

I also changed the include from the std library (<file.h>) to search the /usr/local/include path
by replacing all e.g. <link.h> with "link.h", this repo reflects this.

Copy those files to /usr/local/include/

```bash
cp -frv include/*.h /usr/local/include/
```

```bash
git clone https://github.com/opensource-apple/dtrace.git
mkdir -p obj sym dst
xcodebuild install -target ctfconvert -target ctfdump -target ctfmerge \
ARCHS="x86_64" SRCROOT=$PWD OBJROOT=$PWD/obj SYMROOT=$PWD/sym \
DSTROOT=$PWD/dust
```
