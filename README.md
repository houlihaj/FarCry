# FarCry

This is a 64-bit build tree of the [leaked Far Cry source](https://archive.org/details/far-cry-1.34-complete). While the release bundle was labeled as version 1.34, the version within the code still reads as 1.33 (1.1.3.1395), with the `CryCommon` and `CryGame` components mostly matching the public SDK.

Some modifications were made to original sources and project configuration in order to build using Visual Studio 2022.

This repository has 5 branches:

* __leak__ - contains the original leaked source with project files overwritten by `AMD64_ProjectFiles_VS2005` contents
* __win32_x64__ - contains the original leaked source with project files updated for Visual Studio 2022
* __cleanup__ - contains a work-in-progress code cleanup with minor tweaks for building
* __fixes__ - contains further tweaks and fixes that might not be found upstream
* __Windows11_VS2022__ (default) - branched from fixes; updated for `v14.44 - 17.14` (see **Building from source**)

## Building from source

You will need Visual Studio 2022 with `MSVC v143 C++ x64/x86 build tools` and `C++ MFC for v143 build tools (x86 & x64)` components. (Note: `v14.44 - 17.14` was used to build this project for Win11; April 2026)

Just load `Game01.sln`, select `Release64` solution configuration and run `Build solution`. 20 out of 22 projects will compile and link, however 2 will fail: `Editor` and `XRenderOGL`. You can unload them to avoid building them.

## Running the binaries

This x64 build has been tested using assets from game version 1.40, along with additional binary dependencies from [64-bit Upgrade Patch v1.32](https://community.pcgamingwiki.com/files/file/442-far-cry-amd64-64-bit-upgrade-patch/).

1. Create some directory alongside `Bin32` or `Bin64` directories, for example `CustomBuild`
2. Place compiled binaries from `FarCry/x64/Release64/` folder (16 `DLL`s ans 3 `EXE`s in total)
3. You still need to copy the following 5 DLLs from `Bin64` folder: `DivxDecoder.dll`, `DivxMediaLib.dll`, `FileParser.dll`, `msvcr71.dll`, `crysound64.dll`
4. You can optionally copy the `*.pdb` files from `FarCry/x64/Release64/` to aid in debugging

(Note: Game saves are architecture independent and appear to function the same between versions 1.32, 1.33, and 1.40.)

The resulting file tree should look something like this:

```
CustomBuild\
├── Cry3DEngine.dll
├── CryAISystem.dll
├── CryAnimation.dll
├── CryEntitySystem.dll
├── CryFont.dll
├── CryGame.dll
├── CryInput.dll
├── CryMovie.dll
├── CryNetwork.dll
├── CryPhysics.dll
├── CryScriptSystem.dll
├── crysound64.dll
├── CrySoundSystem.dll
├── CrySystem.dll
├── DivxDecoder.dll
├── DivxMediaLib.dll
├── FarCry.exe
├── FarCry_WinSV.exe
├── FileParser.dll
├── msvcr71.dll
├── rc64.exe
├── ResourceCompilerPC64.dll
├── XRenderD3D9.dll
└── XRenderNULL.dll
```

# Editor

The Editor project will build but the Sandbox Editor application needs the game assets to launch. **Editor Requirements** lists some dynamic libraries that are required for **CrySoundSystem.dll** (*d3dx9_43.dll* and *crysound64.dll*) and **XRenderD3D9.dll** (*d3dx9_43.dll*).

## Editor Requirements

1. Install the [DirectX End-User Runtime](https://www.microsoft.com/en-us/download/details.aspx?id=35). In particular, the editor depends on *d3dx9_43.dll*.

2. Install the [Microsoft .NET Framework Version 1.1 Redistributable Package](https://web.archive.org/web/20210517233454/http://www.microsoft.com/en-us/download/details.aspx?id=26). In particular, the editor depends on *msvcr71.dll*. The file is also present with the game assets.

3. The editor depends on *crysound64.dll*, which is found with the game assets.
