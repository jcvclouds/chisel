# Chisel Local Setup
Instructions for setting up your environment to run Chisel locally.

For a minimal setup, you only need to install [SBT (the Scala Build Tool)](http://www.scala-sbt.org), which will automatically fetch the appropriate version of Scala and Chisel based on on your project configuration.

[Firtool](https://github.com/llvm/circt) is required to generate Verilog.

[Verilator](https://www.veripool.org/wiki/verilator) is installation is required to simulate your Verilog designs.

[FileCheck](https://llvm.org/docs/CommandGuide/FileCheck.html) is used by many of the unit tests.
Please see the FileCheck link for documentation about FileCheck's syntax with examples.
Alternatively you can search the tests for examples by searching for `CHECK:` which is used to tell FileCheck to attempt to match the following text in the generated output.

## Ubuntu Linux

1.  Install the JVM
    ```bash
    sudo apt-get install default-jdk
    ```

1.  Install sbt according to the instructions from [sbt download](https://www.scala-sbt.org/download.html).

1.  Install Firtool

    Choose whatever version is being [used in continuous integration](.github/workflows/install-circt/action.yml)
    ```bash
    wget -q -O - https://github.com/llvm/circt/releases/download/firtool-1.56.1/circt-full-shared-linux-x64.tar.gz | tar -zx
    ```
    This will give you a directory called `firtool-1.56.1` containing the firtool binary, add this to your PATH as appropriate.
    ```bash
    export PATH=$PATH:$PWD/firtool-1.56.1/bin
    ```
    Alternatively, you can install the binary to a standard location by simply moving the binary (if you have root access).
    ```bash
    mv firtool-1.56.1/bin/firtool /usr/local/bin/
    ```


1.  Install Verilator.
    We recommend relatively recent verilator
    Follow these instructions to compile it from source.

    1.  Install prerequisites (if not installed already):
        ```bash
        sudo apt-get install git make autoconf g++ flex bison
        ```

    2.  Clone the Verilator repository:
        ```bash
        git clone https://github.com/verilator/verilator
        ```

    3.  In the Verilator repository directory, check out a known good version:
        ```bash
        git pull
        git checkout v5.004
        ```

    4.  In the Verilator repository directory, build and install:
        ```bash
        unset VERILATOR_ROOT # For bash, unsetenv for csh
        autoconf # Create ./configure script
        ./configure
        make
        sudo make install
        ```

1.  Install FileCheck.
    FileCheck can usually be found in llvm-\*-tools packages, eg.
    ```
    sudo apt-get install llvm-12-tools
    export PATH=$PATH:/usr/lib/llvm-12/bin
    ```

    You can alternatively download a statically-ish linked binary from https://github.com/jackkoenig/FileCheck
    ```
    mkdir filecheck
    cd filecheck
    wget -q https://github.com/jackkoenig/FileCheck/releases/download/FileCheck-16.0.6/FileCheck-linux-x64
    mv FileCheck-linux-x64 FileCheck
    chmod +x FileCheck
    export PATH=$PATH:$PWD
    ```
    Similarly to firtool, you can install the binary to a more standard location by moving it.
    ```
    mv FileCheck /usr/local/bin
    ```

## Arch Linux
1.  Install Verilator and SBT
    ```bash
    pacman -Sy verilator sbt
    ```

1. Install firtool

    See the instructions for Ubuntu above, the firtool Ubuntu binary is a "many Linux" mostly statically linked binary.

## Windows
1.  [Download and install sbt for Windows](https://www.scala-sbt.org/download.html).

Verilator does not appear to have native Windows support.
However, Verilator works in [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) or in other Linux-compatible environments like Cygwin.

There are no issues with generating Verilog from Chisel, which can be pushed to FPGA or ASIC tools.

## Mac OS X
1.  Install Verilator and SBT
    ```bash
    brew install sbt verilator
    ```

1.  Install firtool

    ```bash
    wget -q -O - https://github.com/llvm/circt/releases/download/firtool-1.56.1/circt-full-shared-macos-x64.tar.gz | tar -zx
    ```
    This will give you a directory called `firtool-1.56.1` containing the firtool binary, add this to your PATH as appropriate.
    ```bash
    export PATH=$PATH:$PWD/firtool-1.56.1/bin
    ```
    Alternatively, you can install the binary to a standard location by simply moving the binary.
    ```bash
    mv firtool-1.56.1/bin/firtool /usr/local/bin/
    ```

1.  Install FileCheck.
    You can download a statically-ish linked binary from https://github.com/jackkoenig/FileCheck
    ```
    mkdir filecheck
    cd filecheck
    wget -q https://github.com/jackkoenig/FileCheck/releases/download/FileCheck-16.0.6/FileCheck-macos-x64
    mv FileCheck-macos-x64 FileCheck
    chmod +x FileCheck
    export PATH=$PATH:$PWD
    ```
    Similarly to firtool, you can install the binary to a more standard location by moving it.
    ```
    mv FileCheck /usr/local/bin
    ```
