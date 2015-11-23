###
# config
###
cmake_minimum_required(VERSION 2.8.1)
cmake_policy(SET CMP0042 NEW)

###
# verbose make
###
# set(CMAKE_VERBOSE_MAKEFILE TRUE)


###
# project
###
set(PROJECT cpp_redis)
project(${PROJECT} CXX)


###
# compilation options
###
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -W -Wall -Wextra")


###
# variables
###
set(DEPS_FOLDER ${PROJECT_SOURCE_DIR}/deps)
set(GTEST_INCLUDES ${DEPS_FOLDER}/gtest/include)
set(GTEST_LIBS ${DEPS_FOLDER}/gtest/lib)
set(CPP_REDIS_INCLUDES ${PROJECT_SOURCE_DIR}/includes)


###
# includes
###
include_directories(${CPP_REDIS_INCLUDES})


###
# link
###
link_directories(${GTEST_LIBS})


###
# sources
###
set(DIRS "sources" "sources/network" "sources/builders" "sources/replies")
foreach(dir ${DIRS})
  # get directory sources
  file(GLOB s_${dir} "${CMAKE_CURRENT_SOURCE_DIR}/${dir}/*.cpp")
  # set sources
  set(SOURCES ${SOURCES} ${s_${dir}})
endforeach()

find_package(Boost REQUIRED COMPONENTS system)

###
# executable
###
add_library(${PROJECT} STATIC ${SOURCES})
target_link_libraries(${PROJECT} ${Boost_LIBRARIES})


###
# install
###
install (TARGETS ${PROJECT} DESTINATION lib)
install (DIRECTORY ${CPP_REDIS_INCLUDES}/ DESTINATION include)

###
# examples
###
IF (BUILD_EXAMPLES)
    add_subdirectory(examples)
ENDIF(BUILD_EXAMPLES)


###
# tests
###
IF (BUILD_TESTS)
    add_subdirectory(tests)
ENDIF(BUILD_TESTS)
