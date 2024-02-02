# Modern CMake

## Written on Feb 02 2024

> While reading Modern CMake for C++
> [Buy](https://www.packtpub.com/product/modern-cmake-for-c/9781801070058)

### Mastering the CMake executable's CLI
- Syntax of the _buildsystem_ generation.
    ```bash
    $ cmake -B ./build -S ./project # source tree in `project`
    ```
    > The `-S` is optional. Not providing `-B` will result in a
    > messy in-source build.
- Selecting and configuring which build tool to use for building
 is handled by CMake. It can by overriden as below.
    ```bash
    $ cmake -G <generator-name> <path-to-source>
    ```
- The `CMAKE_BUILD_TYPE` variable can take values: `Debug`,
 `Release`, `MinSizeRel` or `RelWithDebInfo`. This would generate
 a build tree specific to the build type. Default is `Debug`.
- There are multiple levels to filter the log outputs - `ERROR`,
 `WARNING`, `NOTICE`, `STATUS`, `VERBOSE`, `DEBUG` or `TRACE`. By
 default it is set to `STATUS`.
- Instead of running `make` after generation of build tree to build
 the project, it is recommended to do the following instead.
    ```bash
    $ cmake --build <dir>   # bare minimum
    $ cmake --build <dir> -j [<number-of-jobs>] # for parallel builds
    $ cmake --build <dir> -t clean  # remove build artifacts with `clean` target
    $ cmake --build <dir> --clean-first # clean first and then build
    $ cmake --build <dir> -v    # to obtain more detailed logs
    ```
- Once a project is built, users can install it on their system, _i.e._,
 copy files to correct directories, install libraries, etc.
    ```bash
    $ cmake --install <dir> # bare minimum
    $ cmake --install <dir> --component <comp>  # install only one component
    ```



## Written on Jan 30 2024

> While reading Modern CMake for C++
> [Buy](https://www.packtpub.com/product/modern-cmake-for-c/9781801070058)


### What are the requirements for the script?
- Automate building process to go through project tree and compiles everything.
- Avoid redundant compilations, _i.e._, check whether the source has been modified
 since the last time we ran it.
- Easily manage compiler arguments for each file.
- Build whole solutions that can be resued in bigger projects.

### What are the different aspects of building C++ software?
- Compiling executables and libraries
- Managing dependencies
- Testing
- Installing
- Packaging
- Producing documentation
- Testing some more :wink:

### Why CMake?
- Supports modern compilers and toolchains.
- Cross-platform - Windows, Linux, macOS.
- Generate project files for popular IDEs.
- Operates on right level of abstraction.
- Philosophy that testing, packaging and installing are inherent part
 of the build process.
- Old features get deprecated, resulting in _lean_ CMake.
- Continuous Integration / Continuous Deployment (CI/CD) can use the
 same CMake configuration.

### How does CMake work?
- At an abstract level - it reads source code and produces binaries.
 However, it relies on other tools to perform the actual compilation,
 linking and other tasks.
- It is the _orchestrator_ of the building process: knows what steps
 need to done, what the end goal is, and how to find the right tools
 and materials for the job.

![stages-cmake.png](assets/stages-cmake.png)

- There are three stages:
    1. **Configuration**
        - Read project details stored in _source tree_ directory and
         and prepare _build tree_ directory for the generation stage.
        - Start with emtpy build tree, collect details about the working
         environment - architecture, available compilers, linkers, etc.
        - `CMakeLists.txt` file is parsed and executed. This file is the
         bare minimum of a CMake project. Tells CMake about project
         structure, its targets and its dependencies.
        - CMake stores collected information in the build tree which
         is used for the next step.
        - `CMakeCache.txt` is created to store more variables (path
         to compilers and other tools) and save time during the next
         configuration.
    2. **Generation**
        - After the above step, CMake will generate a **buildsystem**
         for the exact working environment it is working in.
        - _Buildsystems_ are simply cut-to-size configuration files
         for other build tools (Makefiles for GNU Make _or_ Ninja).
        - The generation stage is executed automatically after the
         configuration stage. To explicitly run only the configuration
         stage, you can use the `cmake-gui` utility.
    3. **Building**
        - In this stage, we run the appropriate _build tool_.
        - The build tool will execute steps to produce **targets** with
         compilers, linkers, static and dynamic analysis tools, test
         frameworks, etc.
- CMake is able to produce buildsystems on demand for every platform
 with a single configuration, _i.e._, same project files.

> Commands to build a very simple application:
>
    > cmake -B build
    >
    > cmake --build build
>

Corresponding `CMakeLists.txt`:
```cmake
cmake_minimum_required(VERSION 3.20)
project(hello)
add_executable(hello hello.cpp)
```

- The above example, generates a buildsystem that is stored in the
 `build` directory, executes the build stage and produces a final
 binary that you can run.
