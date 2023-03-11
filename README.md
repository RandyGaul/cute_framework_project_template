# Cute Framework Project Template

A copy + pastable CMake project template for [Cute Framework](https://github.com/RandyGaul/cute_framework). Here are the steps to get a project off the ground and building.

1. Download and install CMake v3.14+ (for easy cross-platform building), and [git](https://git-scm.com/downloads). 
2. Copy CMakeLists.txt ([this one here](https://github.com/RandyGaul/cute_framework_project_template/blob/main/CMakeLists.txt)) into the top-level of your project directory.
3. Find + replace "my_project_name".
4. Run CMake on your project folder.

That's it! Feel free to skip the rest of this page if you're comfortable. The rest of this page is a CMake 101 walkthrough for those new to C/C++ or CMake.

## CMake 101 Walkthrough

If you're a bit new to C/C++ then using CMake and getting started may be a bit difficult. Here's a CMake 101 tutorial to help newcomers get going. Be sure to follow the above steps until #4 -- this section will help you do #4.

CMake is a cross-platform tool to generate a build setup. It doesn't actually build your code for you. For Windows users most people will use CMake to generate Visual Studio solution files. For MacOS users most people will generate an XCode project. CMake will automatically find your preferred compiler/build tools and use them. The reason CMake is used in CF, is that it's pretty much the only option available today for cross-platform C/C++ that actually works well. It's the current industry standard. CMake sort of sucks, and is admittedly "baggage", but we more or less have to deal with it to get started.

### Building your Project

Here are the steps to build your game.

1. Download and install CMake v3.14+ (for easy cross-platform building), and [git](https://git-scm.com/downloads). 
2. Copy CMakeLists.txt ([this one here](https://github.com/RandyGaul/cute_framework_project_template/blob/main/CMakeLists.txt)) into the top-level of your project directory.
3. Find + replace "my_project_name".
4. Open a command prompt in your folder (terminal for MacOS users).
5. Create a folder called `build_folder`. This is where cmake will store your generated build files.
6. Run this command: `cmake -A x64 -Bbuild_folder .`

And that's it! Your build project has been generated. You can now build your game by using the build project inside of `build_folder`. If you generated for Visual Studio you `.sln` file is in `build_folder`. If you used XCode then your XCode project is in `build_folder`.

### Specific Build Systems

You can pick a specific build system with the `-G` command. For example, on Windows we can pick a specific version of Visual Studio by using slightly different CMake commands. Here are some examples to generate differnt build folders for different versions of Visual Studio.

* `cmake -G "Visual Studio 14 2015" -A x64 -Bbuild_msvc_2015 .`
* `cmake -G "Visual Studio 15 2017" -A x64 -Bbuild_msvc_2017 .`
* `cmake -G "Visual Studio 16 2019" -A x64 -Bbuild_msvc_2019 .`
* `cmake -G "Visual Studio 17 2022" -A x64 -Bbuild_msvc_2022 .`

You can also generate your build for makefiles, or even other types of build systems. [Here's a bunch of information about various kinds of builds](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html) if you're not using Visual Studio.

### CMakeLists.txt Details

Here are some notes on the important parts of our CMakeLists.txt file:

* Under `add_executable` you can list out all of your source code.
* You can toggle between using Cute Framework as a static or dynamic library by toggling this cmake switch `set(CUTE_FRAMEWORK_STATIC ON)` from ON to OFF.
* `target_include_directories` tells cmake where your source code is. The template CMakeLists.txt file looks in the `src` folder.
* The rest of the cmake file tells cmake where to download a copy of CF, and other basic settings.

## Full Game Example

If you're stuck I highly recommend downloading a copy of [Cute Snake](https://github.com/RandyGaul/cute_snake) to see as a reference. For Windows users you can simply click on the [msvc2019.cmd](https://github.com/RandyGaul/cute_snake/blob/master/msvc2019.cmd) batch file if you're using Visual Studio 2019, and the script is trivial to modify for other Visual Studio versions.

## Ask for Help

If this seems overwhelming or you still need help I recommend asking for help here in a [GitHub Issue](https://github.com/RandyGaul/cute_framework/issues), or hopping onto the [CF discord server](https://discord.gg/2DFHRmX) to ask for help.
