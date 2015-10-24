cmake_minimum_required(VERSION 2.6)

project(Quacks)

find_package(Qt5Widgets)

include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}/src
  ${CMAKE_CURRENT_BINARY_DIR}
  ${Qt5Widgets_INCLUDE_DIRS})

macro(append_prefix srcs)
  foreach(src IN LISTS ${srcs})
    if(IS_ABSOLUTE ${src})
      list(APPEND tmp_list ${src})
    else()
      list(APPEND tmp_list ${CMAKE_CURRENT_LIST_DIR}/${src})
    endif()
  endforeach()
  set(${srcs} ${tmp_list} )
  unset(tmp_list)
endmacro()

if(MSVC)
  set_target_properties(Quakcs PROPERTIES LINK_FLAGS "/SUBSYSTEM:WINDOWS")
  set(exe_option WIN32)
elseif(APPLE)
  set(exe_option MACOSX_BUNDLE)
endif()

include(src/CMakeLists.txt)
set(TARGET_NAME Quacks)
if(ANDROID)
  add_library(${TARGET_NAME} SHARED ${exe_option} ${QUACKS_MAIN_SRCS})
else()
  add_executable(${TARGET_NAME} ${exe_option} ${QUACKS_MAIN_SRCS})
endif()

set_target_properties(${TARGET_NAME} PROPERTIES VERSION 0.0.1)
target_link_libraries(${TARGET_NAME} Qt5::Widgets)

if(ANDROID)
  include(${CMAKE_CURRENT_SOURCE_DIR}/toolchain/android/AddQtAndroidApk.cmake)
  add_qt_android_apk(QuacksAPK ${TARGET_NAME})
endif()
