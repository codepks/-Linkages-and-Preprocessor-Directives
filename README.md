source of this entire page is [here](https://github.com/methylDragon/coding-notes/blob/master/C++/08%20C++%20-%20Linkages%20and%20Preprocessor%20Directives.md)

# C++ Build Process

**Flow**
Editor <--> Pre-processor <--> Compiler <--> Linker <--> Dynamic library Loader <--> Input Output Executer

## Compilation steps
1. **Pre-processing** : ```#include``` , ```#define``` and other preprocessor directives. In prepocessing these are completely replaced by some form of text and forms a **completely parseable C++ file**.
2. **Compilation** : Compiles **each source file** separately into assembly code and forms **object file**.
3. **Linking** : Linker takes the object file from above and forms either a library file(.lib or .dll) or an executable file. The linking instrcutions are given in CMakeLists.txt

## .lib or .dll and .exe files

### .lib
- Contains only compiled code in form of functions, variables, etc. but doesn't include an **entry point (main function)**
- Only meant for static linking i.e. code from ```.lib``` file is copied and integrated in .exe files
- a ```.lib``` files can be created alognside a .dll file to be linked to a program to resolve references issue during compilation
- **CREATION** : C or C++ files is compiled into .obj files and a **linker tool** links this object files resolved references like extern too. The output of linker tool is saved in form of **.lib** files
- On **Visual Studio** one can condfigure project to create a .lib files and on **Command Line Tools** one can use ```cl(compiler)``` and ```lib(linker)``` to do the same



### .dll
- Meant for dynamic loading
- The key difference between .lib and .dll file is that .dll files have **exported functions** defined which makes them accessible and are used by other programs
- The function export works generally using ```__declspec(dllexport)```
- ```DLLMain``` is an entry point to dlls

```
#ifdef _WIN32

// Define dllexport for Windows
#define DLL_EXPORT __declspec(dllexport)

#else

// Define dllexport for other platforms (optional)
#define DLL_EXPORT

#endif

DLL_EXPORT int add(int a, int b) {
  return a + b;
}
```

**DLLMain**
- It is an optional entry point to a DLL
- When a DLL is loaded using ```LoadLibrary``` and unloads using ```FreeLibrary``` , it utilizes ```DLLMain```

*Sample code*

```
BOOL WINAPI DllMain(

HINSTANCE hinstDLL, //A handle to the DLL instance itself.

DWORD fdwReason,   //Flag indicating reason for call
                   //DLL_PROCESS_ATTACH: Process initialization or LoadLibrary call.
                   //DLL_THREAD_ATTACH: Thread creation.
                   //DLL_THREAD_DETACH: Thread termination.
                   //DLL_PROCESS_DETACH

LPVOID lpvReserved); //Reserved parameter, usually set to NULL.
```




### .exe
- Contains normal code + necessary instructions and resources to launch on its own
- It contains the **entry point - the main function** which is required for the program to launch


# Creating a .dll 

**Source Code**
```
#include <windows.h>

// Define the export macro for portability
#ifdef _WIN32
#define DLL_EXPORT __declspec(dllexport)
#else
#error "This code is currently only supported for Windows systems."
#endif

// Function to add two numbers
DLL_EXPORT int Add(int a, int b) {
    return a + b;
}

// Optional DllMain function
BOOL WINAPI DllMain(HINSTANCE hinstDLL, DWORD fdwReason, LPVOID lpvReserved) {
    switch (fdwReason) {
        case DLL_PROCESS_ATTACH:
            // Perform any process-level initialization here if needed
            break;
        case DLL_PROCESS_DETACH:
            // Perform any process-level cleanup here if needed
            break;
        case DLL_THREAD_ATTACH:
        case DLL_THREAD_DETACH:
            // Perform any thread-level initialization/cleanup here if needed
            break;
    }

    return TRUE; // Indicate successful execution
}

// Add a main function for testing purposes can be removed in production
int main() {
    int x = 5, y = 10;
    int result = Add(x, y);
    std::cout << "Result: " << result << std::endl;
    return 0;
}
```
- ```#include <windows.h>```: This header is essential for Windows API functions like DllMain.
- ```DLL_EXPORT``` marks the Add function for exporting.
- 

**Compilation**

```
cl /EHsc /Fe:add_dll.dll add_dll.cpp /link user32.lib kernel32.lib
```
- In your project settings, configure the output type as "DLL" and link the necessary libraries (e.g., ```kernel32.lib``` ).
- Replace ```/EHsc``` with the appropriate exception handling model for your project.
- Adjust the library paths (```user32.lib``` and ```kernel32.lib```) if necessary

# Linkages
source is [here](https://www.goldsborough.me/c/c++/linker/2016/03/30/19-34-25-internal_and_external_linkage_in_c++/)

Linkers look into the aspect of internal and external linkages before linking the object files. Following are the keywords and their linkge behaviour:
1) **static** : internal linkage
2) **extern** : external linkage
3) **non-const global variables** : external linkage
4) **const global variables** : internal linkage
5) **functions** : external

> Explaination <br>
> - **Translation Units** : It means all the .cpp / .c files and header files .h/.hpp files it includes
> - **Internal Linkage**  : These are visible to the _linker_ within that translation units
> - **External Linkage**  : _Linker_ can see it when processing other translation units too
