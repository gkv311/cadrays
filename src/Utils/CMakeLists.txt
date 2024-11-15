cmake_minimum_required (VERSION 3.13)

project (Utils)

set (USED_SOURCES
  Settings.cxx
  ini.c
)
set (USED_HEADERS
  Settings.hxx
  ini.h
)
source_group ("Source Files" FILES ${USED_SOURCES})
source_group ("Header Files" FILES ${USED_HEADERS})

add_library (${PROJECT_NAME} STATIC ${USED_SOURCES} ${USED_HEADERS})
set_target_properties (${PROJECT_NAME} PROPERTIES INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}")
