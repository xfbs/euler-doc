# Library

All shared code is placed in an external library, `libeulerutil`. This contains useful functions and data structures.

The code for this library exists at <https://github.com/xfbs/libeulerutil>. This includes the library code itself, as well as a number of tests.

## Building

To build this library, you can check it out using `git` and then uses `meson` and `ninja` to build it.The library has no external dependencies.

```bash
$ git clone https://github.com/xfbs/libeulerutil
$ cd libeulerutil
$ meson build
$ cd build
$ ninja
```

By default, this will also build the tests. It is advisable to run them.

```bash
$ ./tests
```

You can install the library into your local system.

```bash
ninja install
```
