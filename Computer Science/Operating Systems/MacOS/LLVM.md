# Installing **LLVM** Using Homebrew on macOS (as following an article)

## Introduction

**LLVM** *(Low-Level Virtual Machine)* is a collection of modular and reusable compiler and toolchain technologies.
It is a powerful framework that enables developers to create compilers and languages that optimize machine code for performance, portability, and efficiency.

If you are working on **macOS** and want to install **LLVM** for development purposes, 
using **Homebrew** is one of the simplest and most efficient methods. **Homebrew** is a popular package manager for macOS that simplifies the installation of software.In this article, we will walk through the process of installing LLVM using Homebrew, covering everything from installation requirements to verifying your LLVM installation. By the end, you will be equipped with the knowledge to set up LLVM on your macOS system seamlessly.2. 

## Prerequisites

Before we proceed with the installation steps, make sure you have the following prerequisites:

- macOS: LLVM can be installed on macOS versions that support Homebrew.
- Homebrew: Ensure that you have Homebrew installed. If you haven’t installed it yet, you can do so by running the following command in your terminal: /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" 
- Command Line Tools for Xcode: You will also need the Command Line Tools. You can install them using: xcode-select --install

## Installing **LLVM** with Homebrew

Once you have ensured the prerequisites are met, follow these steps to install LLVM using Homebrew:

1. Update HomebrewIt’s a good practice to update Homebrew to ensure you have the latest version of the package definitions. Run the following command in your terminal:brew update
2. Search for **LLVM** Versions. Homebrew allows you to install different versions of LLVM. To find available versions, use this command:brew search llvmThis command will display a list of available LLVM versions, such as llvm, llvm@10, llvm@11, etc. For this guide, we’ll focus on the latest version, which is typically just llvm.
3. 3: Install LLVMNow that you know the name of the package, you can install LLVM. To install the latest version, run:brew install llvmIf you need a specific version of LLVM, you can specify it like this:brew install llvm@10
4. 4: Configure Your EnvironmentAfter installation, it’s important to add LLVM’s binaries to your system’s PATH. This step allows you to use the LLVM tools directly from the command line. You can do this by adding the following lines to your shell configuration file (e.g., .bash_profile, .zshrc, or .bashrc):
   ```bash
export PATH="/usr/local/opt/llvm/bin:$PATH"
```
5. After modifying your shell configuration file, load the changes by running:
   ```bash
   source ~/.bash_profile 
   # or source ~/.zshrc
   ```
6. Verifying the Installation. 
   To verify that **LLVM** has been successfully installed, you can check the version of clang, the C/C++ compiler that comes with LLVM. Run the following command:
   ```bash
   clang --version
   ```
   You should see output indicating the version of Clang that is part of your LLVM installation.

## Using LLVM

With **LLVM** installed, you can now take full advantage of its capabilities. Here are some common tools and commands you can use: 
- Compile ***C/C++*** Code: You can compile a ***C or C++*** source file using the clang compiler. For example:
  ```bash
  clang main.c -o main
  ```
- View **LLVM Intermediate Representation**: To generate the LLVM IR from a C source file, use the -S -emit-llvm flags:
  ```bash
  clang -S -emit-llvm main.c -o main.ll
  ```
- Use **llvm-dis** to Convert Bitcode to LLVM IR: If you have an **LLVM bitcode** file (.bc), you can convert it back to **LLVM IR**:
  ```bash
  llvm-dis example.bc5.
  ```

## Conclusion

Installing **LLVM** on macOS using Homebrew is straightforward and efficient. With the steps outlined in this guide, you should now have a working installation of LLVM, ready for development. By understanding the various tools and commands at your disposal, you can leverage LLVM’s capabilities to enhance your programming projects.Whether you are a seasoned developer or just starting with compilers, **LLVM** provides the foundation for a wealth of powerful applications and optimizations.

Don’t hesitate to explore further and dive into the extensive documentation available on the **LLVM** website to enhance your understanding and skills.

# After install DEV steps

```bash
CLANG_CONFIG_FILE_SYSTEM_DIR: /opt/homebrew/etc/clang
CLANG_CONFIG_FILE_USER_DIR:   ~/.config/clang

LLD is now provided in a separate formula:
  brew install lld

Using `clang`, `clang++`, etc., requires a CLT installation at `/Library/Developer/CommandLineTools`.
If you don't want to install the CLT, you can write appropriate configuration files pointing to your
SDK at ~/.config/clang.

To use the bundled libunwind please use the following LDFLAGS:
  LDFLAGS="-L/opt/homebrew/opt/llvm/lib/unwind -lunwind"

To use the bundled libc++ please use the following LDFLAGS:
  LDFLAGS="-L/opt/homebrew/opt/llvm/lib/c++ -L/opt/homebrew/opt/llvm/lib/unwind -lunwind"

NOTE: You probably want to use the libunwind and libc++ provided by macOS unless you know what you're doing.

llvm is keg-only, which means it was not symlinked into /opt/homebrew,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have llvm first in your PATH, run:
  echo 'export PATH="/opt/homebrew/opt/llvm/bin:$PATH"' >> ~/.zshrc

For compilers to find llvm you may need to set:
  export LDFLAGS="-L/opt/homebrew/opt/llvm/lib"
  export CPPFLAGS="-I/opt/homebrew/opt/llvm/include"
```