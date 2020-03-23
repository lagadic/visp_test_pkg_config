project(test-pkg-config)

cmake_minimum_required(VERSION 3.16)

find_package(PkgConfig QUIET)
pkg_check_modules(PC_VISP visp)

set(CATCH_INCLUDE_DIRECTORIES ${CMAKE_CURRENT_LIST_DIR}/3rdparty/catch2)

#message("PC_VISP_INCLUDE_DIRS: ${PC_VISP_INCLUDE_DIRS}")
#message("PC_VISP_LIBRARIES: ${PC_VISP_LIBRARIES}")
#message("PC_VISP_LINK_LIBRARIES: ${PC_VISP_LINK_LIBRARIES}")
#message("PC_VISP_CFLAGS: ${PC_VISP_CFLAGS}")
#message("PC_VISP_LIBRARY_DIRS: ${PC_VISP_LIBRARY_DIRS}")

file(GLOB_RECURSE src_cpp RELATIVE ${CMAKE_CURRENT_LIST_DIR} "*.cpp")

#set(src_cpp_to_remove testRand.cpp)

#foreach(src ${src_cpp})
#  foreach(src_rm ${src_cpp_to_remove})
#    if(${src} MATCHES "${src_rm}")
#      message("on a trouve ${src_rm} dans ${src}")
#      list(REMOVE_ITEM src_cpp ${src})
#    endif()
#  endforeach()
#endforeach()


#message("src_cpp: ${src_cpp}")

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

#include_directories(${PC_VISP_INCLUDE_DIRS} ${CATCH_INCLUDE_DIRECTORIES})
#link_directories(${PC_VISP_LIBRARY_DIRS})

foreach(src ${src_cpp})
  # Compute the name of the binary to create
  get_filename_component(binary ${src} NAME_WE)

  # From source compile the binary and add link rules
  add_executable(${binary} ${src})
  target_include_directories(${binary} PUBLIC ${PC_VISP_INCLUDE_DIRS} ${CATCH_INCLUDE_DIRECTORIES})
  target_link_libraries(${binary} ${PC_VISP_LIBRARIES})
  target_link_directories(${binary} PUBLIC ${PC_VISP_LIBRARY_DIRS})
endforeach()

