# Cute Framework Project Template

A copy + pastable CMake project template for [Cute Framework](https://github.com/RandyGaul/cute_framework). Here are the steps to get a project off the ground and building.

1. Download and install CMake (v3.14 or higher, you can just get the latest version). CMake is for easy cross-platform building. Also install [git](https://git-scm.com/downloads). If you're new to git it's highly recommended to use [Github Desktop](https://desktop.github.com/).
2. Copy CMakeLists.txt ([this one here](https://github.com/RandyGaul/cute_framework_project_template/blob/main/CMakeLists.txt)) into the top-level of your project directory.
3. Find + replace "mygame" with the name of your game (no spaces, special characters, or underscores allowed).
4. Make a folder called `src` in the top-level of your project, and place your initial `main.cpp` there. You can use [main.cpp](https://github.com/RandyGaul/cute_framework_project_template/blob/main/src/main.cpp) from this repo (the code as a snippet is just a little ways down in this guide, you can copy + paste it).
5. Run CMake on your project folder. If you need help with this step, try reading our [CMake 101 section](https://github.com/RandyGaul/cute_framework_project_template#cmake-101-walkthrough) (just down a bit on this page, scroll down).

That's it! Feel free to skip the rest of this page if you're comfortable. The rest of this page is a CMake 101 walkthrough for those new to C/C++ or CMake.

You can use this code snippet for your initial `main.cpp`.

```cpp
#include <cute.h>
using namespace Cute;

int main(int argc, char* argv[])
{
	// Create a window with a resolution of 640 x 480.
	int options = APP_OPTIONS_DEFAULT_GFX_CONTEXT | APP_OPTIONS_WINDOW_POS_CENTERED;
	Result result = make_app("Fancy Window Title", 0, 0, 0, 640, 480, options, argv[0]);
	if (is_error(result)) return -1;

	while (app_is_running())
	{
		app_update();
		// All your game logic and updates go here...
		app_draw_onto_screen();
	}

	destroy_app();

	return 0;
}
```

## CMake 101 Walkthrough

If you're a bit new to C/C++ then using CMake and getting started may be a bit difficult. Here's a CMake 101 tutorial to help newcomers get going. Be sure to follow the above steps until #4 -- this section will help you do #5.

CMake is a cross-platform tool to generate a build setup. It doesn't actually build your code for you. For Windows users most people will use CMake to generate Visual Studio solution files. For MacOS users most people will generate an XCode project. CMake will automatically find your preferred compiler/build tools and use them. The reason CMake is used in CF, is that it's pretty much the only option available today for cross-platform C/C++ that actually works well. It's the current industry standard. CMake sort of sucks, and is admittedly "baggage", but we more or less have to deal with it to get started.

### Building your Project

Here are the steps to build your game.

1. Download and install CMake v3.14+ (for easy cross-platform building), and [git](https://git-scm.com/downloads). 
2. Copy CMakeLists.txt ([this one here](https://github.com/RandyGaul/cute_framework_project_template/blob/main/CMakeLists.txt)) into the top-level of your project directory.
3. Find + replace "mygame" with the name of your game (no spaces, special characters, or underscores allowed).
4. Make a folder called `src` in the top-level of your project, and place your initial `main.cpp` there.
5. Open a command prompt in your folder (terminal for MacOS users).
6. Create a folder called `build_folder`. This is where cmake will store your generated build files.
7. Run this command: `cmake -A x64 -Bbuild_folder .`

And that's it! Your build project has been generated. You can now build your game by using the build project inside of `build_folder`. If you generated for Visual Studio you `.sln` file is in `build_folder`. If you used XCode then your XCode project is in `build_folder`.

### Specific Build Systems

You can pick a specific build system with the `-G` command. For example, on Windows we can pick a specific version of Visual Studio by using slightly different CMake commands. Here are some examples to generate differnt build folders for different versions of Visual Studio.

* `cmake -G "Visual Studio 14 2015" -A x64 -Bbuild_msvc_2015 .`
* `cmake -G "Visual Studio 15 2017" -A x64 -Bbuild_msvc_2017 .`
* `cmake -G "Visual Studio 16 2019" -A x64 -Bbuild_msvc_2019 .`
* `cmake -G "Visual Studio 17 2022" -A x64 -Bbuild_msvc_2022 .`

You can also generate your build for makefiles, or even other types of build systems. [Here's a bunch of information about various kinds of builds](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html) if you're not using Visual Studio. Here are some popular ones: "-G "Unix Makefiles", "-G Xcode", "-G MSYS Makefiles", "-G MinGW Makefiles".

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
