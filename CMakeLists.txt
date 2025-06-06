# Stop if cmake version is below 3.0
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)

# Project name and enable C++ support
project(hellocone LANGUAGES CXX)

# Require C++11
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

if(POLICY CMP0072)
  # Get rid of OpenGL GLVND warning from CMake 3.11
  cmake_policy(SET CMP0072 NEW)
endif()

# Fail early if one of the required packages cannot be found
find_package(OpenGL REQUIRED)
include(FindGTK2)
find_package(GTK2 REQUIRED gtk)
include(FindCoin)
find_package(Coin REQUIRED)
include(FindSoGtk)
find_package(SoGtk REQUIRED)

find_package(PkgConfig REQUIRED)

pkg_check_modules (
  GTK_GL2    # variable will be used by cmake
  REQUIRED   # is required mandatory
  gtkgl-2.0  # name of package to be found
)

# Define the executable
add_executable(hellocone "")

# Define the sources
target_sources(
  hellocone
  PRIVATE
    src/HelloCone/HelloCone.cpp
  )

# Specify the include directories
#include_directories(/usr/include/gtk-2.0 /usr/include/glib-2.0 /usr/lib/x86_64-linux-gnu/glib-2.0/include /usr/include/cairo /usr/include/pango-1.0 /usr/include/harfbuzz)
if(GTK2_FOUND)
  include_directories(${Coin_INCLUDE_DIRS} ${SoGtk_INCLUDE_DIRS} ${GTK2_INCLUDE_DIRS})
  target_link_libraries(hellocone ${Coin_LIBRARIES} ${SoGtk_LIBRARIES} ${GTK2_LIBRARIES})
endif()
