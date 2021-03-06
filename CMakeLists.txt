# Copyright (c) 2011 Daniel Pfeifer <daniel@pfeifer-mail.de>
#               2011-2014 Stefan.Eilemann@epfl.ch

cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
project(vmmlib)

set(VERSION_MAJOR 1)
set(VERSION_MINOR 8)
set(VERSION_PATCH 0)
set(VERSION_ABI 3)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/CMake
                              ${CMAKE_SOURCE_DIR}/CMake/common)
include(GitExternal)
include(Common)
include(FindPackages)

if(CMAKE_COMPILER_IS_GNUCXX)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-shadow -Wno-sign-compare -Wno-extra -Wno-strict-aliasing")
endif()

set(DOCS README.md AUTHORS LICENSE.txt ACKNOWLEDGEMENTS)

configure_file(${PROJECT_SOURCE_DIR}/vmmlib/version.in.hpp
  ${OUTPUT_INCLUDE_DIR}/vmmlib/version.hpp @ONLY)

set(HEADERS
  ${OUTPUT_INCLUDE_DIR}/vmmlib/version.hpp
  vmmlib/aabb.hpp
  vmmlib/enable_if.hpp
  vmmlib/exception.hpp
  vmmlib/frustum.hpp
  vmmlib/frustum_culler.hpp
  vmmlib/intersection.hpp
  vmmlib/lowpass_filter.hpp
  vmmlib/math.hpp
  vmmlib/matrix.hpp
  vmmlib/matrix_functors.hpp
  vmmlib/matrix_pseudoinverse.hpp
  vmmlib/matrix_traits.hpp
  vmmlib/qr_decomposition.hpp
  vmmlib/quaternion.hpp
  vmmlib/util.hpp
  vmmlib/validator.hpp
  vmmlib/vector.hpp
  vmmlib/vector_traits.hpp
  vmmlib/visibility.hpp
  vmmlib/vmmlib.hpp
  vmmlib/vmmlib_config.hpp
)

add_subdirectory(tests)
add_subdirectory(tests_old)

install(FILES ${HEADERS} DESTINATION include/vmmlib COMPONENT dev)
install(FILES ${DOCS} DESTINATION share/vmmlib COMPONENT dev)

if(LAPACK_FOUND)
  install(FILES ${LAPACK_HEADERS} DESTINATION include/vmmlib/lapack/detail
    COMPONENT dev)
endif()

include(DoxygenRule) # must be after all targets
include(CPackConfig)
include(CTest)
include(PackageConfig)
