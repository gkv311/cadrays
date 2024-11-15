cmake_minimum_required (VERSION 3.13)

project (ImGui)

set (USED_SOURCES
  ImColorPicker.cxx
  imgui.cxx
  imgui_demo.cxx
  imgui_draw.cxx
  ImGuizmo.cxx
  ImRaytraceControls.cxx
)
set (USED_HEADERS
  stb/stb_rect_pack.h
  stb/stb_textedit.h
  stb/stb_truetype.h
  IconsFontAwesome.h
  ImColorPicker.h
  imconfig.h
  imgui.h
  imgui_dock.h
  imgui_internal.h
  imgui_user.h
  ImGuizmo.h
  ImRaytraceControls.h
  imgui_dock.inl
  imgui_user.inl
)
source_group ("Source Files" FILES ${USED_SOURCES})
source_group ("Header Files" FILES ${USED_HEADERS})

add_library (${PROJECT_NAME} SHARED ${USED_SOURCES} ${USED_HEADERS})
set_target_properties (${PROJECT_NAME} PROPERTIES INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}")

set (anOcctLibs TKV3d TKService TKPrim TKTopAlgo TKGeomAlgo TKBRep TKGeomBase TKG3d TKG2d TKMath TKernel)
target_link_libraries (${PROJECT_NAME} PRIVATE ${anOcctLibs})

add_dependencies      (${PROJECT_NAME}         Utils)
target_link_libraries (${PROJECT_NAME} PRIVATE Utils)

install (TARGETS ${PROJECT_NAME} RUNTIME DESTINATION "${CMAKE_INSTALL_PREFIX}")
