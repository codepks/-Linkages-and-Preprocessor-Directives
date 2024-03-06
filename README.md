> source : https://bit.ly/49F00x4 <br>
> C++/08 C++ - Linkages and Preprocessor Directives.md

# C++ Build Process

**Flow**
Editor <--> Pre-processor <--> Compiler <--> Linker <--> Dynamic library Loader <--> Input Output Executer

## Compilation steps
1. **Pre-processing** : ```#include``` , ```#define``` and other preprocessor directives. In prepocessing these are completely replaced by some form of text and forms a **completely parseable C++ file**.
2. **Compilation** : Compiles each source file separately into assembly code and forms **object file**.
3. **Linking** : Linker takes the object file from above and forms either a library file(.lib or .dll) or an executable file

## .lib or .dll and .exe files

### .lib
- Contains only compiled code in form of functions, variables, etc. but doesn't include an **entry point (main function)**
- Only meant for static linking i.e. code from .lib file is copied and integrated in .exe files
- a C or C++ files is compiled into .obj files and a **linker tool** links this object files resolved references like extern too. The output of linker tool is saved in form of **.lib** files
- On **Visual Studio** one can condfigure project to create a .lib files and on **Command Line Tools** one can use ```cl(compiler)``` and ```lib(linker)``` to do the same


### .dll
- 

### .exe
- Contains normal code + necessary instructions and resources to launch on its own
- It contains the **entry point - the main function** which is required for the program to launch
