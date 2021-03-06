
include_directories(${CMAKE_SOURCE_DIR}/include)





# - Create interface target for local catch copy
# Single header is placed in dedicated directory for clean separation
# Using an interface target means we simply link to it like any other target
# and CMake with add all needed definitions, include paths etc.
# To update the vendored copy of catch, simply download it via:
#
# $ curl -O https://raw.githubusercontent.com/philsquared/Catch/master/single_include/catch.hpp
#
# and copy the `catch.hpp` file into the catch subdirectory
add_library(catch INTERFACE)
target_include_directories(catch INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/catch)

#find_package(Eigen3)


# - Create and define test executables
# Here a generic example for creating an executable then defining it as a
# test to CTest
# 1. Create executable
add_executable(test_hit_class test_hit_class.cpp)

# 2. Link it to the library being tested, plus the unit test library(ies)
target_link_libraries(test_hit_class  catch)

# 3. Add the test program as a command to be run as a test
# Note: NAME is what will be shown in CTest, COMMAND is what will be run
#       CMake knows that text_example is a CMake target, and knows where
#       the resultant executable is. Things like PATH and so on do not need
#       to be set.
# Note: The executable can be passed command line arguments. Here we add
#       `--success` which is provided by Catch's main() function and reports
#       info about successful as well as failing tests.
add_test(NAME testHitClass COMMAND test_hit_class --success)


add_executable(test_integration test_integration.cpp)
target_link_libraries(test_integration  catch)
add_test(NAME testIntegration COMMAND test_integration --success)

add_executable(test_cmgraph_class test_cmgraph_class.cpp)
target_link_libraries(test_cmgraph_class  catch)
add_test(NAME testCMGraph COMMAND test_cmgraph_class --success)

add_executable(test_fkdtree test_fkdtree.cpp)
target_link_libraries(test_fkdtree  catch)
add_test(NAME testFKDTree COMMAND test_fkdtree --success)

add_executable(test_riemannfit test_riemannfit.cpp)
include_directories( $ENV{EIGEN_INCLUDE_DIR})
target_link_libraries(test_riemannfit  catch)
add_test(NAME testRiemannFit COMMAND test_riemannfit --success)
