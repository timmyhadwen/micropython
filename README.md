[![Build Status][travis-img]][travis-repo] [![Coverage Status][coveralls-img]][coveralls-repo] [![Issue Stats][istats-pr-img]][istats-pr-repo] [![Issue Stats][istats-issue-img]][istats-issue-repo]
[travis-img]:  https://travis-ci.org/micropython/micropython.png?branch=master
[travis-repo]: https://travis-ci.org/micropython/micropython
[coveralls-img]:  https://coveralls.io/repos/micropython/micropython/badge.png?branch=master
[coveralls-repo]: https://coveralls.io/r/micropython/micropython?branch=master
[istats-pr-img]: http://issuestats.com/github/micropython/micropython/badge/pr
[istats-pr-repo]: http://issuestats.com/github/micropython/micropython
[istats-issue-img]: http://issuestats.com/github/micropython/micropython/badge/issue
[istats-issue-repo]: http://issuestats.com/github/micropython/micropython

The Micro Python project
========================
<p align="center">
  <img src="https://raw.githubusercontent.com/micropython/micropython/master/logo/upython-with-micro.jpg" alt="MicroPython Logo"/>
</p>

This is the Micro Python project, which aims to put an implementation
of Python 3.x on microcontrollers and small embedded systems.

WARNING: this project is in beta stage and is subject to changes of the
code-base, including project-wide name changes and API changes.

Micro Python implements the entire Python 3.4 syntax (including exceptions,
"with", "yield from", etc.).  The following core datatypes are provided:
str (including basic Unicode support), bytes, bytearray, tuple, list, dict,
set, frozenset, array.array, collections.namedtuple, classes and instances.
Builtin modules include sys, time, and struct.  Note that only subset of
Python 3.4 functionality implemented for the data types and modules.

See the repository www.github.com/micropython/pyboard for the Micro
Python board, the officially supported reference electronic circuit board.

Major components in this repository:
- py/ -- the core Python implementation, including compiler, runtime, and
  core library.
- unix/ -- a version of Micro Python that runs on Unix.
- stmhal/ -- a version of Micro Python that runs on the Micro Python board
  with an STM32F405RG (using ST's Cube HAL drivers).
- minimal/ -- a minimal Micro Python port. Start with this if you want
  to port Micro Python to another microcontroller.

Additional components:
- bare-arm/ -- a bare minimum version of Micro Python for ARM MCUs. Used
  mostly to control code size.
- teensy/ -- a version of Micro Python that runs on the Teensy 3.1
  (preliminary but functional).
- pic16bit/ -- a version of Micro Python for 16-bit PIC microcontrollers.
- cc3200/ -- a version of Micro Python that runs on the CC3200 from TI.
- esp8266/ -- an experimental port for ESP8266 WiFi modules.
- tests/ -- test framework and test scripts.
- tools/ -- various tools, including the pyboard.py module.
- examples/ -- a few example Python scripts.
- docs/ -- official documentation in RST format.

"make" is used to build the components, or "gmake" on BSD-based systems.
You will also need bash and Python (at least 2.7 or 3.3).

The Unix version
----------------

The "unix" port requires a standard Unix environment with gcc and GNU make.
x86 and x64 architectures are supported (i.e. x86 32- and 64-bit), as well
as ARM and MIPS. Making full-featured port to another architecture requires
writing some assembly code for the exception handling and garbage collection.
Alternatively, fallback implementation based on setjmp/longjmp can be used.

To build (*):

    $ cd unix
    $ make

Then to give it a try:

    $ ./micropython
    >>> list(5 * x + y for x in range(10) for y in [4, 2, 1])

Learn about command-line options (in particular, how to increase heap size
which may be needed for larger applications):

    $ ./micropython --help

Run complete testsuite:

    $ make test

Unix version comes with a builtin package manager called upip, e.g.:

    $ ./micropython -m upip install micropython-pystone
    $ ./micropython -m pystone

Browse available modules on
[PyPI](https://pypi.python.org/pypi?%3Aaction=search&term=micropython).
Standard library modules come from
[micropython-lib](https://github.com/micropython/micropython-lib) project.

(*) Debian/Ubuntu/Mint derivative Linux distros will require build-essentials,
libffi-dev and pkg-config packages installed. If you have problems with some
dependencies, they can be disabled in unix/mpconfigport.mk .

The STM version
---------------

The "stmhal" port requires an ARM compiler, arm-none-eabi-gcc, and associated
bin-utils.  For those using Arch Linux, you need arm-none-eabi-binutils and
arm-none-eabi-gcc packages from the AUR.  Otherwise, try here:
https://launchpad.net/gcc-arm-embedded

To build:

    $ cd stmhal
    $ make

You then need to get your board into DFU mode.  On the pyboard, connect the
3V3 pin to the P1/DFU pin with a wire (on PYBv1.0 they are next to each other
on the bottom left of the board, second row from the bottom).

Then to flash the code via USB DFU to your device:

    $ make deploy

You will need the dfu-util program, on Arch Linux it's dfu-util-git in the
AUR.  If the above does not work it may be because you don't have the
correct permissions.  Try then:

    $ sudo dfu-util -a 0 -d 0483:df11 -D build-PYBV10/firmware.dfu

Building the documentation locally
----------------------------------

Install Sphinx, and optionally (for the RTD-styling), sphinx_rtd_theme,
preferably in a virtualenv:

     pip install sphinx
     pip install sphinx_rtd_theme

In `micropython/docs`, build the docs:

    make MICROPY_PORT=<port_name> BUILDDIR=<port_name>/build html

Where `<port_name>` can be `unix`, `pyboard`, `wipy` or `esp8266`.

You'll find the index page at `micropython/docs/<port_name>/build/html/index.html`.
