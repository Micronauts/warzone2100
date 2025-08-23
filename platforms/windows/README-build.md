# Building Warzone 2100 for Windows

### Getting the Source

- Clone the Git repo:
  ```shell
  git clone https://github.com/Warzone2100/warzone2100.git
  cd warzone2100
  git fetch --tags
  git submodule update --init --recursive
  ```
  > Note: Initializing submodules is required.

Do **not** use GitHub's "Download Zip" option, as it **does not contain submodules** or the Git-based autorevision information.

* **Building from the command-line:**
   1. Starting from the _parent_ directory of the warzone2100 repository, create an [out-of-source](https://cmake.org/cmake/help/book/mastering-cmake/chapter/Getting%20Started.html#directory-structure) build directory:
      ```shell
      mkdir build
      ```
   2. Change directory into the `build` directory:
      ```shell
      cd build
      ```
   3. Run CMake configure to generate the build files:
      ```shell
      cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCMAKE_INSTALL_PREFIX:PATH=~/wz/install -GNinja ../warzone2100
      ```
      > - [Modify the `CMAKE_INSTALL_PREFIX` parameter value as desired](https://cmake.org/cmake/help/latest/variable/CMAKE_INSTALL_PREFIX.html) to configure the base installation path.
      > - The `../warzone2100` path at the end should point to the warzone2100 repo directory. This example assumes that the repo directory and the build directory are siblings, and that the repo was cloned into a directory named `warzone2100`.
   4. Run CMake build:
      ```shell
      cmake --build . --target install
      ```


## Building with MSVC:

### Prerequisites:

1. **Visual Studio 2022** (Visual Studio 2019 may work, but 2022+ is strongly encouraged)
    - If you do not already have Visual Studio installed, you can download the free **Visual Studio Community** from: https://developer.microsoft.com/en-us/windows/downloads
    - IMPORTANT: You need the fully-featured Visual Studio IDE. “Visual Studio Code” does not include the necessary support for building C++ Windows apps.
2. **CMake 3.28+**
    - If you do not have CMake 3.24+, you can [download the latest stable version for free at CMake.org](https://cmake.org/download/#latest).
3. **Git** (if not building from a release source archive)
4. **7-Zip** (https://www.7-zip.org)
5. **Vulkan SDK 1.2.148.1+** (https://vulkan.lunarg.com/sdk/home)
  - Required only if you want to build with Vulkan support.



### Preparing to build:

Build dependencies are provided via [vcpkg](https://github.com/Microsoft/vcpkg) from Microsoft.
* Run the `get-dependencies_win.ps1` script from powershell inside of the repo root directory in order to download and build the dependencies.

> Enter `.\get-dependencies_win.ps1` in the powershell developer console to run it. If you lack permissions and entering `Get-ExecutionPolicy` returns `Restricted`, use `Set-ExecutionPolicy RemoteSigned`.

### Building from the command-line:
1. Change directory to the warzone2100 repo directory
2. Configure
    * Visual Studio 2022: `cmake -H. -DCMAKE_TOOLCHAIN_FILE=vcpkg\scripts\buildsystems\vcpkg.cmake -Bbuild -G "Visual Studio 17 2022"`
    * Visual Studio 2019: `cmake -H. -DCMAKE_TOOLCHAIN_FILE=vcpkg\scripts\buildsystems\vcpkg.cmake -Bbuild -G "Visual Studio 16 2019"`
3. Build
    * Release: `cmake --build build --config Release`
    * Debug: `cmake --build build --config Debug`
    
### Building using Visual Studio:
 1. Open Visual Studio
 2. Open the warzone2100 folder using **File** > **Open** > **Folder...**
    - Allow Visual Studio some time to load the project and retrieve information from CMake.
 3. Create a VS CMake settings JSON file using **CMake** > **Change CMake settings**. You can also reach this dialog by clicking "Manage Configurations" in the configuration dropdown in the toolbar. Make sure the CMake components in Visual Studio are installed (by running the Visual Studio Installer).
    - This creates `CMakeSettings.json`
 4. Add the following variables to `CMakeSettings.json`:
    - To `cmakeCommandArgs`, add: `-DCMAKE_TOOLCHAIN_FILE=vcpkg\scripts\buildsystems\vcpkg.cmake`
    - Note: Visual Studio automatically escapes and turns each `\` into `\\`
 5. After letting Visual Studio re-run CMake configure with the new settings, you can build using the **CMake** menu.
