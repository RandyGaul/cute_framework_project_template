cmake_minimum_required(VERSION 3.14)
project(my_project_name)

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
	my_project_name
	src/main.cpp
)

# Our source code will be in the `src` folder.
target_include_directories(my_project_name PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>)

if(EMSCRIPTEN)
	set(CMAKE_EXECUTABLE_SUFFIX ".html")
	target_compile_options(my_project_name PUBLIC -O1 -fno-rtti -fno-exceptions)
	target_link_options(my_project_name PRIVATE -o my_project_name.html --preload-file ${CMAKE_SOURCE_DIR}/content@/content --emrun -s ASYNCIFY=1 -O1)
endif()

# Make the game link against Cute Framework.
target_link_libraries(my_project_name cute)

# For convenience on Windows, set MSVC debugger's working directory in the build folder.
# Also ask MSVC to make my_project_name the startup project.
if (MSVC)
	set_property(TARGET my_project_name PROPERTY VS_DEBUGGER_WORKING_DIRECTORY $<TARGET_FILE_DIR:my_project_name>)
	set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT my_project_name)
endif()
