cmake_minimum_required(VERSION 3.14)

include(cmake/prelude.cmake)

project(
    cmake_init
    VERSION 0.1.0
    DESCRIPTION "Short description"
    HOMEPAGE_URL "https://example.com/"
    LANGUAGES CXX
)

include(cmake/project-is-top-level.cmake)
include(cmake/variables.cmake)

# ---- Declare library ----

add_library(
    cmake_init_lib OBJECT
    source/lib.cpp
)

target_include_directories(
    cmake_init_lib ${warning_guard}
    PUBLIC
    "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/source>"
)

target_compile_features(cmake_init_lib PUBLIC cxx_std_17)

# ---- Declare executable ----

add_executable(cmake_init_exe source/main.cpp)
add_executable(cmake_init::exe ALIAS cmake_init_exe)

set_property(TARGET cmake_init_exe PROPERTY OUTPUT_NAME cmake_init)

target_compile_features(cmake_init_exe PRIVATE cxx_std_17)

target_link_libraries(cmake_init_exe PRIVATE cmake_init_lib)

# ---- Install rules ----

if(NOT CMAKE_SKIP_INSTALL_RULES)
  include(cmake/install-rules.cmake)
endif()

# ---- Developer mode ----

if(NOT cmake_init_DEVELOPER_MODE)
  return()
elseif(NOT PROJECT_IS_TOP_LEVEL)
  message(
      AUTHOR_WARNING
      "Developer mode is intended for developers of cmake_init"
  )
endif()

include(cmake/dev-mode.cmake)