cmake_minimum_required(VERSION 3.0)
project(test)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -frtti")
find_package(GTest REQUIRED)
find_package(yaml-cpp REQUIRED)

include_directories(${GTEST_INCLUDE_DIRS})

set(ethercat_LIB /usr/local/lib/libethercat.so)

#################################################
# Different ways of finding the shared library
#################################################
set(ethercat_interface_lib /home/naci/ethercat_interface/lib/ethercat_interface/libethercat_interface.so)
set(ethercat_interface_INCLUDE_DIRS /home/naci/ethercat_interface/include/)
add_library(libethercat_interface SHARED IMPORTED)
set_target_properties(libethercat_interface PROPERTIES IMPORTED_LOCATION ${ethercat_interface_lib})

get_filename_component(PARENT_DIR ../ REALPATH)
#set(ethercat_interface_lib PARENT_DIR/lib/ethercat_interface/libethercat_interface.so)

#message(STATUS ${PARENT_DIR})

add_executable(single_slave_test single_slave_test/single_slave_test.cpp)
target_link_libraries(single_slave_test libethercat_interface ${ethercat_LIB} ${GTEST_LIBRARIES} ${YAML_CPP_LIBRARIES} pthread)
target_include_directories(single_slave_test PUBLIC ${PARENT_DIR}/include ${YAML_CPP_INCLUDE_DIRS})

add_executable(multiple_identical_slaves_test multiple_identical_slaves_test/multiple_identical_slaves_test.cpp)
target_link_libraries(multiple_identical_slaves_test libethercat_interface ${GTEST_LIBRARIES} ${YAML_CPP_LIBRARIES} pthread)
target_include_directories(multiple_identical_slaves_test PUBLIC ${PARENT_DIR}/include ${YAML_CPP_INCLUDE_DIRS})

add_executable(timer_test timer_test/timer_test.cpp)
target_link_libraries(timer_test libethercat_interface ${ethercat_LIB} pthread)
target_include_directories(timer_test PUBLIC ${PARENT_DIR}/include)

add_executable(shared_data_test shared_data_test/shared_data_test.cpp)
target_link_libraries(shared_data_test libethercat_interface ${GTEST_LIBRARIES} ${YAML_CPP_LIBRARIES} pthread)
target_include_directories(shared_data_test PUBLIC ${PARENT_DIR}/include)