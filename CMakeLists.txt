cmake_minimum_required(VERSION 2.8.3)
project(glkh_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

set(VERSION 1.0)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

set(GKLH_VERSION 5.0.3)
ExternalProject_Add(glkh
  URL http://webhotel4.ruc.dk/~keld/research/GLKH/GLKH-${GKLH_VERSION}.tgz
  UPDATE_COMMAND ""
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX}
    -DCMAKE_BUILD_TYPE:STRING=Release
  BUILD_COMMAND $(MAKE)
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} glkh)

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
          CFG_EXTRAS glkh-extras.cmake)
cs_install()
