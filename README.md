# OpenSolver Solvers Installer

Builds a .pkg installer for OpenSolver solver binaries for Mac Office 2016.

## Why do we need an installer?

Office for Mac 2016 is sandboxed, meaning that executables can only be run if they are located in a set of whitelisted directories on the computer. We need to place the Solvers directory into one of these whitelisted locations so that we can run the solver binaries for OpenSolver.

Apple's list of whitelisted directories is [here](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AppSandboxInDepth/AppSandboxInDepth.html#//apple_ref/doc/uid/TP40011183-CH3-SW17). However note that on OS X 10.11 and higher, these listed directories are protected by [System Integrity Protection](https://support.apple.com/en-us/HT204899) and cannot be modified, even by root. It turns out that the `/Library` directory is also whitelisted (despite not being listed as such) and can be modified, although it does require root permission to be modified.

Our solution to the sandbox problem is to copy the OpenSolver `Solvers` folder to `/Library/OpenSolver/Solvers` so that the binaries can be executed inside the sandbox. As noted earlier, copying to `/Library` will require root permission and some copying that is non-trivial to some users, so we use this installer to streamline the setup process and put the solvers in the correct place just by double-clicking the installer and entering their password.

## How does it work?

This .pkg installer is built using the [Packages](http://s.sudre.free.fr/Software/Packages/about.html) software, which makes it easy to create a simple installer.

The only configuration in the Packages file is to specify the Payload (`../OpenSolver/Solvers`), the Destination (`/Library/OpenSolver/Solvers`) and to exclude the `win32` and `win64` subdirectories from the Payload.

Building the project creates the `OpenSolver Solvers.pkg` file in the `build` folder. This is self-contained and can be distributed as-is. It will need to be updated whenever the packaged solvers in OpenSolver are changed/updated.
