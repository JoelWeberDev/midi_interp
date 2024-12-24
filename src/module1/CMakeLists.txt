# Define the module1 library
add_library(module1
    STATIC
    src/module1.cpp
)

# Specify include directories for module1
target_include_directories(module1 PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")