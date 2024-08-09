cmake_minimum_required(VERSION 3.12)
project(pybintest_simple)

include_directories(${MY_BASE_DIR})

# Find the pybind11 package
add_subdirectory(pybind11)

# Create the module
pybind11_add_module(pybind_wrapper MODULE pybind_wrapper.cpp)

# Find the ROOT package
find_package(ROOT REQUIRED)

# Include the necessary directories
include_directories(${ROOT_INCLUDE_DIRS})

# Link the ClassicSVfit library
target_link_libraries(pybind_wrapper PUBLIC ${MY_BASE_DIR}/TauAnalysis/ClassicSVfit/lib/libTauAnalysis_ClassicSVfit.so)
