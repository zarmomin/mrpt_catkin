cmake_minimum_required(VERSION 2.8.3)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")

project(mrpt_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

# need to explicitly pass the SuiteSparse stuff so mrpt finds one that is compatible with the rest of the workspace
ExternalProject_Add(mrpt_src
  GIT_REPOSITORY git@github.com:zarmomin/mrpt.git
  GIT_TAG master
  UPDATE_COMMAND ""
  PATCH_COMMAND patch -p1 < ${CMAKE_CURRENT_SOURCE_DIR}/patches/mainly-yaml.patch
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
    -DCMAKE_BUILD_TYPE=Release
    -DBUILD_SHARED_LIBS=ON
    -Dsuitesparse_guidance_dir=${suitesparse_DIR}
  BUILD_COMMAND make -j8
  INSTALL_COMMAND make install -j8
)

cs_install()

cs_export(CFG_EXTRAS mrpt-extras.cmake)
