# Stop if cmake version is below 3.0
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)

# Project name and enable C++ support
project(hellocone LANGUAGES CXX)

# Require C++11
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Fail early if one of the required packages cannot be found
find_package(OpenGL REQUIRED)
include(FindGTK2)
find_package(GTK2 REQUIRED gtk)

find_package(PkgConfig REQUIRED)

pkg_check_modules (
  GTK_GL2    # variable will be used by cmake
  REQUIRED   # is required mandatory
  gtkgl-2.0  # name of package to be found
)

# Define the executable
add_executable(hellocone.x "")

# Define the sources
target_sources(
  hellocone.x
  PRIVATE
    src/HelloCone/HelloCone.cpp
  )

# Specify the include directories
include_directories(/usr/include/gtk-2.0 /usr/include/glib-2.0 /usr/lib/x86_64-linux-gnu/glib-2.0/include /usr/include/cairo /usr/include/pango-1.0 /usr/include/harfbuzz)
