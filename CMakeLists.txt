cmake_minimum_required(VERSION 3.5.0)
project(midi_interp VERSION 0.1.0 LANGUAGES C CXX)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 17)

# External libraries using opencv as an example
# find_package(OpenCV REQUIRED)

# Define the main executable
add_executable(${PROJECT_NAME} main.cpp)

# include the external libraries include files
# target_include_directories(${PROJECT_NAME} PRIVATE ${OpenCV_INCLUDE_DIRS})

# Link the main executable with the external libraries opencv is an example
# target_link_libraries(${PROJECT_NAME} PRIVATE ${OpenCV_LIBS})

# Function for importing libraries
# Please Note that library directories should be in the following format
# libs
# ├── lib1
# │   ├── include
# │   │   └── lib1_file.h
# │   └── src
# │       ├── lib1_file.cpp
# │       └── ...
# Particulariy no CMakeLists.txt file in the lib directory
function(add_libs_from_subdirectories base_dir)
    file(GLOB subdirs LIST_DIRECTORIES true ${base_dir}/*)
    foreach(subdir ${subdirs})
        if(IS_DIRECTORY ${subdir})
            get_filename_component(lib_name ${subdir} NAME)
            file(GLOB_RECURSE sources ${subdir}/src/*.cpp)
            add_library(${lib_name} STATIC ${sources})
            target_include_directories(${lib_name} PUBLIC "${subdir}/include")
            target_link_libraries(${PROJECT_NAME} PRIVATE ${lib_name})
        endif()
    endforeach()
endfunction()

# Function for adding subdirectories which contain CMakeLists.txt
function(add_subdirectories base_dir)
    file(GLOB subdirs LIST_DIRECTORIES true ${base_dir}/*)
    foreach(subdir ${subdirs})
        if(IS_DIRECTORY ${subdir})
            get_filename_component(lib_name ${subdir} NAME)
            add_subdirectory(${subdir})
            target_link_libraries(${PROJECT_NAME} PRIVATE ${lib_name})
        endif()
    endforeach()
endfunction()

# Add directories from src
add_subdirectories(${CMAKE_CURRENT_SOURCE_DIR}/src)

# Add libraries from libs
add_libs_from_subdirectories(${CMAKE_CURRENT_SOURCE_DIR}/libs)

### Testing ###
# Uncomment the following lines to enable testing
# # Enable testing
# enable_testing()

# # Add the tests directory
# add_subdirectory(tests)