# Find GTest
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

message(STATUS "GTest include directory: ${GTEST_INCLUDE_DIRS}")

message(STATUS "Path to GTest libs: ${GTEST_LIBRARIES}")

# Define test executable
add_executable(test1 test1.cpp)

# Link the main executable with the GTest libraries
target_link_libraries(test1 GTest::gtest GTest::gtest_main)
target_link_libraries(test1 ${GTest_LIBRARIES} pthread)

# Link test executable with the main project (if needed)
# target_link_libraries(test1 PRIVATE ${PROJECT_NAME})

# Add tests
add_test(NAME Test1 COMMAND test1)