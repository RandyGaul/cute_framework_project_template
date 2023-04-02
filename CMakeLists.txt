cmake_minimum_required(VERSION 3.14)
project(mygame)

# Must have at least C++17.
set(CMAKE_CXX_STANDARD 17)

if(CMAKE_SYSTEM_NAME MATCHES "Emscripten")
	set(EMSCRIPTEN TRUE)
endif()

# Make sure all binaries are placed into the same build folder.
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

# Link your project against CF statically, rather than as a shared library.
set(CUTE_FRAMEWORK_STATIC ON)

# This will download and build Cute Framework just once the first time you build your game.
include(FetchContent)
FetchContent_Declare(
	cute
	GIT_REPOSITORY https://github.com/RandyGaul/cute_framework
)
FetchContent_MakeAvailable(cute)

# Source code for your game.
add_executable(
	mygame
	src/main.cpp
)

# Our source code will be in the `src` folder.
target_include_directories(mygame PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>)

# Support for web builds through the Emscripten compiler.
if(EMSCRIPTEN)
	set(CMAKE_EXECUTABLE_SUFFIX ".html")
	target_compile_options(mygame PUBLIC -O1 -fno-rtti -fno-exceptions)
	target_link_options(mygame PRIVATE -o mygame.html --preload-file ${CMAKE_SOURCE_DIR}/content@/content --emrun -s ASYNCIFY=1 -O1)
endif()

# Some basic information needed for CMake to generate your Info.plist file.
# This is necessary for e.g. iOS builds.
if(APPLE)
	set_target_properties(mygame PROPERTIES
		MACOSX_BUNDLE_GUI_IDENTIFIER "com.myteam.mygame"
		MACOSX_BUNDLE_BUNDLE_VERSION "1.0.0"
		MACOSX_BUNDLE_SHORT_VERSION_STRING "1.0.0"
	)
endif()

# Make the game link against Cute Framework.
target_link_libraries(mygame cute)

# For convenience on Windows, set MSVC debugger's working directory in the build folder.
# Also ask MSVC to make the game the startup project.
if (MSVC)
	set_property(TARGET mygame PROPERTY VS_DEBUGGER_WORKING_DIRECTORY $<TARGET_FILE_DIR:mygame>)
	set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT mygame)
endif()
