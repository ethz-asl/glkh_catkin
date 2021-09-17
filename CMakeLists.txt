cmake_minimum_required(VERSION 2.8.3)
project(glkh_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

set(VERSION 1.0)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

set(GKLH_VERSION 1.0)
ExternalProject_Add(glkh
  URL http://webhotel4.ruc.dk/~keld/research/GLKH/GLKH-${GKLH_VERSION}.tgz
  URL_MD5 9b0ba92053dac798f550c5c8a9524120
  PATCH_COMMAND
    COMMAND cp ${PROJECT_SOURCE_DIR}/patches/CMakeLists.txt ./
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX}
    -DCMAKE_BUILD_TYPE:STRING=Release
    -DBUILD_SHARED_LIBS=ON
    -DCMAKE_INSTALL_BINDIR=${CATKIN_PACKAGE_BIN_DESTINATION}
    -DCMAKE_INSTALL_LIBDIR=${CATKIN_PACKAGE_LIB_DESTINATION}
  BUILD_COMMAND $(MAKE)
  INSTALL_COMMAND $(MAKE) install
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} glkh)

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
          CFG_EXTRAS glkh-extras.cmake)
cs_install()
