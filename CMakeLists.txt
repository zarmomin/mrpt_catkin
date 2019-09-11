cmake_minimum_required(VERSION 2.8.3)
project(mrpt_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(mrpt_src
  GIT_REPOSITORY git@github.com:zarmomin/mrpt.git
  GIT_TAG nscheidt/ros-fix
  UPDATE_COMMAND ""
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
    -DCMAKE_BUILD_TYPE=Release
    -DBUILD_SHARED_LIBS=ON
  BUILD_COMMAND make -j8
  INSTALL_COMMAND make install -j8
)

#set(mrpt_INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include/mrpt)

cs_install()

cs_export(CFG_EXTRAS mrpt-extras.cmake)
