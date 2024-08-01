cmake_minimum_required(VERSION 3.28...3.30)

project(
	mygame
	VERSION 0.0.1
	LANGUAGES CXX
)

# Must have at least C++20.
set(CMAKE_CXX_STANDARD 20)

if(CMAKE_SYSTEM_NAME MATCHES "Emscripten")
	set(EMSCRIPTEN TRUE)
endif()

# Make sure all binaries are placed into the same build folder.
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

# Disable building CF samples by default
set(CF_FRAMEWORK_BUILD_SAMPLES ON)

# Disable building CF tests by default
set(CF_FRAMEWORK_BUILD_TESTS OFF)

# This will download and build Cute Framework just once the first time you build your game.
include(FetchContent)
FetchContent_Declare(
	cute
	GIT_REPOSITORY https://github.com/RandyGaul/cute_framework
	GIT_TAG master
	GIT_SHALLOW
)
FetchContent_MakeAvailable(cute)

# Source code for your game.
add_executable(
	${PROJECT_NAME}
	src/main.cpp
)

# Our source code will be in the `src` folder.
target_include_directories(${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>)

# Support for web builds through the Emscripten compiler.
if(EMSCRIPTEN)
	set(CMAKE_EXECUTABLE_SUFFIX ".html")
	target_compile_options(${PROJECT_NAME} PUBLIC -O1 -fno-rtti -fno-exceptions)
	target_link_options(${PROJECT_NAME} PRIVATE -o ${PROJECT_NAME}.html --preload-file ${CMAKE_SOURCE_DIR}/content@/content --emrun -s ASYNCIFY=1 -O1)
endif()

# Some basic information needed for CMake to generate your Info.plist file.
# This is necessary for e.g. iOS builds.
if(APPLE)
	set_target_properties(
		${PROJECT_NAME}
		PROPERTIES
		MACOSX_BUNDLE_GUI_IDENTIFIER "com.myteam.${PROJECT_NAME}"
		MACOSX_BUNDLE_BUNDLE_VERSION "1.0.0"
		MACOSX_BUNDLE_SHORT_VERSION_STRING "1.0.0"
	)
endif()

# Make the game link against Cute Framework.
target_link_libraries(${PROJECT_NAME} cute)

# For convenience on Windows, set MSVC debugger's working directory in the build folder.
# Also ask MSVC to make the game the startup project.
if (MSVC)
	set_property(TARGET ${PROJECT_NAME} PROPERTY VS_DEBUGGER_WORKING_DIRECTORY $<TARGET_FILE_DIR:${PROJECT_NAME}>)
	set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})
endif()
