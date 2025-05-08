# Set the minimum required version of CMake
cmake_minimum_required(VERSION 3.1)
# Define the project. This is the name of your project and helps organize metadata.
# The project can also define the version and the languages 
#it supports (like CXX for C++).
project("My little project")
# How to use this CMake file:
# Run the following commands in your terminal to generate 
# a Makefile and build the project:
# 1. Configure the build system:
#    cmake -G "Unix Makefiles" -B<build_directory> -S<source_directory>
# 2. Build the project:
#    cmake --build <build_directory>

# 3. Optionally, install the project:
#    cmake --install <build_directory>
# Specify include directories for header files. These directories tell the compiler
# where to look for additional header files. 
# Examples:
# - "/opt/local/include" and "/usr/local/include" are common system-wide locations.
# - "${CMAKE_SOURCE_DIR}" refers to the root directory of your project.
include_directories(
    "/opt/local/include"
    "/usr/local/include"
    "${CMAKE_SOURCE_DIR}"
)

# Require C++ 17 for this project. It's important to set the C++ 
#standard *before* defining
# any targets like executables or libraries. The options below enforce the standard:
# - CMAKE_CXX_STANDARD: The desired C++ version (17 in this case).
# - CMAKE_CXX_STANDARD_REQUIRED: Ensures that the compiler enforces this standard.
# - CMAKE_CXX_EXTENSIONS: Disables compiler-specific extensions to ensure portability.
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Let's define some programs to build. Here, we create a list of program 
#names ("foo" and "bar").
# We'll use a loop to define executable targets for each program.
set(programs foo bar)
foreach(prog ${programs})
    # For each program in the list, create an executable using its 
    #corresponding .cpp file.
    add_executable(${prog} ${prog}.cpp)
endforeach()

# Add compiler options to enable extra warnings. This helps catch potential issues 
#during compilation, such as unused variables or implicit conversions.
add_compile_options(-Wall -Wextra)

# Build a more complex program, "qux", which depends on multiple source files:
# "qux.cpp" and "otherDependency.cpp".
add_executable(qux qux.cpp otherDependency.cpp)

# Define a static library. Static libraries are linked at compile time and included 
#directly in the final executable. This example creates a static library called 
#"MyIntegrationLibrary" using "myIntegrationLibrary.cpp".
add_library(MyIntegrationLibrary STATIC myIntegrationLibrary.cpp)

# Define a shared library. Shared libraries (or dynamic libraries) are loaded at 
# runtime and can be used by multiple programs.
# This example creates "MyDynamicLib" from "myDynamicLib.cpp".
add_library(MyDynamicLib SHARED myDynamicLib.cpp)

# Link the "qux" executable to the static and shared libraries we just defined.
# This step ensures that "qux" has access to the functions and symbols 
#provided by these libraries.
target_link_libraries(qux PRIVATE MyIntegrationLibrary MyDynamicLib)

# Install targets (executables and libraries) to specific directories. 
#This step is optional but useful if you want to distribute your project. 
#The paths specified here determine where
# the executables, static libraries, and shared libraries will be installed:
# - ARCHIVE DESTINATION: Directory for static libraries (.a files).
# - LIBRARY DESTINATION: Directory for shared libraries (.so or .dll files).
# - RUNTIME DESTINATION: Directory for executables.
install(TARGETS foo bar qux
        ARCHIVE DESTINATION lib/static
        LIBRARY DESTINATION lib/dynamic
        RUNTIME DESTINATION bin)

# Install additional files, such as header files. This ensures that users of 
#your library have access to the headers needed for development. 
#In this example, we install "myPrettyHeader.hpp" to the "include" directory.
install(FILES myPrettyHeader.hpp DESTINATION include)

# Add subdirectories to the project. This is useful for larger projects 
#where each subdirectory has its own CMakeLists.txt file. 
#By calling "add_subdirectory", CMake will process these files as part of the build 
#process. Here, we add "this" and "that" subdirectories.
set(directoriesToProcess this that)
foreach(dir ${directoriesToProcess})
    add_subdirectory(${dir})
endforeach()