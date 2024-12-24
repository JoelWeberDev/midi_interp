add_library(common 
    STATIC 
    src/common.cpp
) 

target_include_directories(common PUBLIC "${CMAKE_CURRENT_LIST_DIR}/include")