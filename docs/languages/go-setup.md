# :simple-go: Go Setup

## 1. System Wide Go with brew

``` { .console .copy }
brew install go --cross-compile-common
```

??? question "ChatGPT answers: what is `cross-compile-common`?"

    1. The `--cross-compile-common` flag adds cross-compilation capabilities. Cross-compiling allows you to build binaries on one platform (like macOS) that can run on another (like Windows, Linux, or ARM architectures).
    2. This option sets up the Go toolchain to include support for various architectures and operating systems out of the box. It’s useful for developers who need to build applications that run on multiple systems without needing a separate build environment for each.
    3. Using cross-compilation saves time, especially in continuous integration (CI) environments where you might need binaries for several platforms