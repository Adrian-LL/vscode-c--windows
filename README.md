# Installing and using Visual Studio Code and g++ (plus other tools) for (re)learning programming (and some entertainment purposes) - Part 1 - Windows 10

Recently (actually not so recent...) I wanted to refresh my programming (and logical) skills. So I thought to try some available tools. I was not looking for professional tools, but for something - such an IDE - easy to use, ideally with a lot of help included. (still longing for something like the almost forgotten Turbo C and Turbo Pascal.)

I tried (and still using somewhat) **CodeBlocks** (http://www.codeblocks.org/ - works on Linux, Windows, MacOS). 
For educational purposes it is very simple to setup, one have to just download and install the version that contains the mingw tools. More details on their site. It is also easy to use, almost straightforward. Nonetheless, it can also deal with bigger projects.

I also tried **NetBeans** (https://netbeans.org/ - also working in Linux, Windows and MacOS) and **DevC++** (https://sourceforge.net/projects/orwelldevcpp/ - Windows only), plus a short stint with **CLion** (which I did not liked - maybe because it was overly complicated for what I wanted to do - but that's a personal thing). Speaking of which, I will not discuss about full-fledged tools such **Visual Studio** (available free in the Community Edition, https://www.visualstudio.com/vs/community/, for Windows and MacOS) or **Xcode** for MacOS, as they seemed too complicated for my purpose. BTW, Visual Studio is huge and I was forced to delete it from my laptop as I did not had enough disk space available after some time.

Please note there are a lot of very good (code) **editors** including ***Notepad++*** (https://notepad-plus-plus.org/), ***atom*** (https://atom.io/), ***Sublime Text*** (https://www.sublimetext.com/) or the ever-faithful ***vim*** (https://www.vim.org/) and ***emacs*** (https://www.gnu.org/software/emacs/). There are even code editors in Microsoft Store (such as ***Code Writer*** https://www.actiprosoftware.com/products/apps/codewriter - which may not be a bad idea).

As I saw a lot of hype lately (that was at the start of 2018) , I wanted to give a chance to Visual Studio Code (https://code.visualstudio.com/). Keep in mind though, although Code can be used for "real" projects, I will refer below mainly to my experience for educational purposes (i.e learning, compiling small console programs and so on.)

> Please note that VSC is still an editor.

Steps I used (the information is already available on the internet, but sometimes there is *too much* information).
> This is the Windows 10 path

## Download & install a build / development environment 

A development environment / set of tools is needed (that is, compiler, debugger, libraries…). For my needs an open-source "toolchain" is enough - usually `MinGW` is most recommended (and you can download it from here: https://sourceforge.net/projects/mingw/). Very familiar for those with Linux experience.

Note however, that the programs resulted are not totally "portable" as they are dependent on some libraries.
Based on my experience:
- MinGW is simple to install, 
- but some tools are missing or not working as they should from the default installation (e.g. make or clang). 
- the installer is ok(ish), but the process of selection is tedious. 
- the uninstall is a little tricky (even while is not actually "installing" something but rather creating some folders and extracting some files there) - but basically means deleting some folder(s) and files.

(Another parenthesis: the fact that there are multiple versions of MinGW, and the discussion about what is Cygwin vs MinGW vs MSYS will be left for some other time… see for example here https://github.com/msys2/msys2/wiki/How-does-MSYS2-differ-from-Cygwin)
> I recommend MSYS2
So, a little more complicated, but somewhat better is the **MSYS2** distribution, as it contains **MSYS2, MinGW-64 & 32** and also **Qt5**. **Clang** is also available. This will need more fine tuning as follows:
- Download it from http://www.msys2.org/. You will find there also the installation instructions. See also the instructions from https://github.com/msys2/msys2/wiki/MSYS2-installation with important remarks about folder names and slightly better pacman commands.

- Please keep the default path for installation (i.e. `C:\`), even try to shorten it as much as possible. For example, `C:\msys64` or `C:\MSYS` - see more in the link above. Just hit "Next".
It will ask to launch the console, proceed to it. Otherwise look from the `msys2_shell.bat` in the `MSYS2` base directory (the one chosen above).

## Now we have to actually install the packages needed for programming.

- Run `pacman -V` in the console (this is the same pacman package manager form Arch Linux). If the version is smaller than 5.0.1 run `update-core` first (but I doubt you will need to use such "older" versions).
- Launch `pacman - Syuu`.
- You will get some errors about catgets - choose `Y`.
- At some moment the console will freeze as it tries to update itself. Just close it (from Windows) and launch again until it does nothing at pacman -Syuu. The update will take a while. The "freeze" may happen also when you do periodical checks for updates (when updating bash or some base libraries)
- Install the build packages.
  - The first one is `pacman -S base-devel`. There will be over 70 packages and more than 200 MB. Just choose `"all"`. This will install basic tools and libraries needed for basic development (you will have `perl` and `python` included, also `(g)awk`, `bison` and so on.)
  - Then install `pacman -S mingw-w64-x86_64-toolchain` for building mingw64 packages. (you can query - see below - and choose the package name). This will have like 40 packages and over 700 MB (deflated). There will be some overhead as not all of them are really needed.
  - Optionally you can install `pacman -S mingw-w64-i686-toolchain` for building mingw32 packages.
  - Also optionally CLANG (search first with `pacman -Ss clang`) `pacman -S mingw64/mingw-w64-x86_64-clang` or Cmake (`pacman -Ss cmake`), `pacman -S mingw64/mingw-w64-x86_64-cmake`

### Some pacman commands useful for General Package Management
- Query about the existence of some package: `pacman - Ss <part_of_package_name>`. It will indicate the correct name for the package and also if it's installed or not. For example, `pacman -Ss gcc`
- Installing new packages: `pacman -S <package_names|package_groups>`
For example, `pacman -S make gettext base-devel`. In this example is a package group which contains many packages. If you try to install a package group, `pacman` will ask you whether you want to install one package from the group or all of the packages from the group.
- Removing packages: `pacman -R <package_names|package_groups>`
- Searching for packages: `pacman -Ss <name_pattern>`

> Beware that mixing in programs from other MSYS2 installations, Cygwin installations, compiler toolchains or even various other programs is not supported and will probably break things in unexpected ways. Do not have these things in PATH when running MSYS2 unless you know what you're doing. ( From < https://github.com/msys2/msys2/wiki/MSYS2-introduction > )

### After MSYS installation:

- Modify the `PATH` system variable in Windows (through "Edit the system variables" in Control Panel - you can edit it system-wide or per-user) to contain the paths for MSYS and MinGW installations.
  - Launch cmd.exe and verify the paths by running `gcc -V` and `g++ -V` (you can try also `gbd`, `make`, `cmake`, etc.)

## Installing Visual Studio Code 

If you din not already installed you can do it now.
- https://code.visualstudio.com/download
- https://code.visualstudio.com/docs/setup/setup-overview
> NOTE - You may get an error about git, let's ignore it for the moment.

- **IMPORTANT** - Install C/C++ (Microsoft) extension (latest version was 0.20.1). This can be triggered by VSC if you enter a small code such as below and saving the file with a `.cpp` extension:

```C++
//please ignore the errors...
#include<iostream>
using namespace std;
int main(){
    cout << "Hello from VSC" << endl;
    return 0;
}

```
This is not a manual for VSC so don't look for detailed explanations, but see below:
- https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools
- https://github.com/Microsoft/vscode-cpptools

## Modify the files needed for VSC

`tasks.json`

`launch.json`

`cpp_properties.json`

## See (some) bibliography below:

C/C++ for VS Code
https://stackoverflow.com/questions/30269449/how-do-i-set-up-visual-studio-code-to-compile-c-code
For example, you may have to modify the include paths for intellisense purposes in VSC cpp_properties.json (NOTE - since the latest update of VSC 1.23.1 and C/C++ extensions I found that the update of that file was not needed.). But see https://github.com/Microsoft/vscode-cpptools/blob/master/Documentation/Getting%20started%20with%20IntelliSense%20configuration.md.

My tasks.json
```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "compile C++",
            "type": "shell",
            "command": "g++ -g ${file} -o ${fileBasenameNoExtension}",
            "presentation": {
                "reveal": "always",
                "panel": "shared"
            },
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```
My launch.json (works well enough for debugging purposes)
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "miDebuggerPath": "C:/msys64/mingw64/bin/gdb.exe",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```
There are better configurations though :-)

Note the use of forward slashes "/" in the paths specifications above - VSC recognizes them. Otherwise one can put escaped backslashes "\\\".

Short remainder of the variables names - see more at https://code.visualstudio.com/docs/editor/variables-reference

```json
${workspaceFolder} - the path of the folder opened in VS Code
${workspaceFolderBasename} - the name of the folder opened in VS Code without any slashes (/)
${file} - the current opened file
${relativeFile} - the current opened file relative to workspaceFolder
${fileBasename} - the current opened file's basename
${fileBasenameNoExtension} - the current opened file's basename with no file extension
${fileDirname} - the current opened file's dirname
${fileExtname} - the current opened file's extension
${cwd} - the task runner's current working directory on startup
${lineNumber} - the current selected line number in the active file
${selectedText} - the current selected text in the active file
Note: The ${workspaceRoot} variable is deprecated in favor of the ${workspaceFolder} variable.
```
