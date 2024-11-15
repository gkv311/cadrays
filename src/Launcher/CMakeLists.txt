cmake_minimum_required (VERSION 3.13)

project (CADRays)

set (USED_SOURCES
  gl3w.c
  OcctLogoPng.c
  tinyfiledialogs.c
  AppConsole.cxx
  AppGui.cxx
  AppViewer.cxx
  CustomWindow.cxx
  DataModelWidget.cxx
  FlightControls.cxx
  GuiBase.cxx
  GuiPanel.cxx
  imgui_impl_glfw_gl3.cxx
  ImportSettingsEditor.cxx
  LightSourcesEditor.cxx
  main.cxx
  MaterialEditor.cxx
  OrbitControls.cxx
  ScriptEditor.cxx
  SettingsWidget.cxx
  TransformWidget.cxx
)
set (USED_HEADERS
  FlightControls.h
  gl3w.h
  imgui_impl_glfw_gl3.h
  OrbitControls.h
  tinyfiledialogs.h
  ViewControls.h
  AppConsole.hxx
  AppGui.hxx
  AppViewer.hxx
  CustomWindow.hxx
  DataModelWidget.hxx
  GuiBase.hxx
  GuiPanel.hxx
  ImportSettingsEditor.hxx
  LightSourcesEditor.hxx
  MaterialEditor.hxx
  ScriptEditor.hxx
  SettingsWidget.hxx
  TransformWidget.hxx
  Version.hxx
)
set (EXE_ICON CADRays.rc)
source_group ("Source Files" FILES ${USED_SOURCES})
source_group ("Header Files" FILES ${USED_HEADERS})
source_group ("Resources"    FILES ${EXE_ICON})

if (WIN32)
  enable_language (RC)
endif()
add_executable (${PROJECT_NAME} ${USED_SOURCES} ${USED_HEADERS} ${EXE_ICON})
add_dependencies      (${PROJECT_NAME}         Utils ImGui ImportExport)
target_link_libraries (${PROJECT_NAME} PRIVATE Utils ImGui ImportExport)

set (anOcctLibs TKViewerTest TKDraw TKOpenGl TKV3d TKService TKPrim TKTopAlgo TKGeomAlgo TKBRep TKGeomBase TKG3d TKG2d TKMath TKernel)
target_link_libraries (${PROJECT_NAME} PRIVATE ${anOcctLibs})

target_include_directories (${PROJECT_NAME} PRIVATE "${TCL_INCLUDE_PATH}")
target_link_libraries (${PROJECT_NAME} PRIVATE ${TCL_LIBRARY})
target_link_libraries (${PROJECT_NAME} PRIVATE glfw)
target_link_libraries (${PROJECT_NAME} PRIVATE ${OPENGL_LIBRARIES})
if (UNIX)
  target_link_libraries (${PROJECT_NAME} PRIVATE dl)
  target_link_libraries (${PROJECT_NAME} PRIVATE pthread)
endif()

# auxiliary development environment
if (MSVC)
  set (3RDPARTY_DLL_DIRS "" CACHE STRING "Paths to external DLLs separated by semicolon (FreeImage, FreeType, Tcl, etc.)")

  # OCCT DLLs
  get_target_property (aTKernelRel "TKernel" IMPORTED_LOCATION_RELEASE)
  get_target_property (aTKernelDbg "TKernel" IMPORTED_LOCATION_DEBUG)
  get_filename_component (OpenCASCADE_BINARY_DIR_RELEASE ${aTKernelRel} DIRECTORY)
  get_filename_component (OpenCASCADE_BINARY_DIR_DEBUG   ${aTKernelDbg} DIRECTORY)
  if (NOT EXISTS "${OpenCASCADE_BINARY_DIR_DEBUG}" AND EXISTS "${OpenCASCADE_BINARY_DIR_RELEASE}")
    set (OpenCASCADE_BINARY_DIR_DEBUG "${OpenCASCADE_BINARY_DIR_RELEASE}")
  elseif (NOT EXISTS "${OpenCASCADE_BINARY_DIR_RELEASE}" AND EXISTS "${OpenCASCADE_BINARY_DIR_DEBUG}")
    set (OpenCASCADE_BINARY_DIR_RELEASE "${OpenCASCADE_BINARY_DIR_DEBUG}")
  endif()

  # Tcl DLLs
  get_filename_component (TCL_BINARY_DIR ${TCL_TCLSH} DIRECTORY)

  # Assimp DLLs
  get_target_property (anAssimpRel assimp::assimp IMPORTED_LOCATION_RELEASE)
  get_target_property (anAssimpDbg assimp::assimp IMPORTED_LOCATION_DEBUG)
  get_filename_component (ASSIMP_BINARY_DIR_RELEASE ${anAssimpRel} DIRECTORY)
  get_filename_component (ASSIMP_BINARY_DIR_DEBUG   ${anAssimpDbg} DIRECTORY)
  if (NOT EXISTS "${ASSIMP_BINARY_DIR_DEBUG}" AND EXISTS "${ASSIMP_BINARY_DIR_RELEASE}")
    set (ASSIMP_BINARY_DIR_DEBUG "${ASSIMP_BINARY_DIR_RELEASE}")
  elseif (NOT EXISTS "${ASSIMP_BINARY_DIR_RELEASE}" AND EXISTS "${ASSIMP_BINARY_DIR_DEBUG}")
    set (ASSIMP_BINARY_DIR_RELEASE "${ASSIMP_BINARY_DIR_DEBUG}")
  endif()
  
  # Visual Studio debugger environment
  set_target_properties(${PROJECT_NAME} PROPERTIES
    VS_DEBUGGER_ENVIRONMENT "\
PATH=%PATH%;$<IF:$<CONFIG:Debug>,${OpenCASCADE_BINARY_DIR_DEBUG};${ASSIMP_BINARY_DIR_DEBUG},${OpenCASCADE_BINARY_DIR_RELEASE};${ASSIMP_BINARY_DIR_RELEASE}>;\
${TCL_BINARY_DIR};${3RDPARTY_DLL_DIRS}\n\
CASROOT=${OpenCASCADE_DIR}\n\
CSF_CadraysDrawTestDefaults=${CMAKE_CURRENT_SOURCE_DIR}/../../data\n\
APP_DATA=${CMAKE_CURRENT_SOURCE_DIR}/../../data/"
  )

  # batch script file running cadrays from make folder
  #configure_file ("${CMAKE_CURRENT_SOURCE_DIR}/cmake/cadrays-release.bat.in" "${CMAKE_CURRENT_BINARY_DIR}/cadrays-release.bat" @ONLY)
  #configure_file ("${CMAKE_CURRENT_SOURCE_DIR}/cmake/cadrays-debug.bat.in"   "${CMAKE_CURRENT_BINARY_DIR}/cadrays-debug.bat"   @ONLY)
endif()

install (TARGETS ${PROJECT_NAME} RUNTIME DESTINATION "${CMAKE_INSTALL_PREFIX}")

# copy data
install (DIRECTORY ${CMAKE_SOURCE_DIR}/data DESTINATION ${CMAKE_INSTALL_PREFIX}
         FILES_MATCHING PATTERN "*.png" PATTERN "*.ttf" PATTERN "DrawPlugin" PATTERN "*.tcl" PATTERN "*.jpg" PATTERN "*.html" PATTERN "*.txt")

install (DIRECTORY ${OpenCASCADE_RESOURCE_DIR}/DrawResources DESTINATION ${CMAKE_INSTALL_PREFIX}/occt-tcl/src)
install (DIRECTORY ${OpenCASCADE_RESOURCE_DIR}/Shaders       DESTINATION ${CMAKE_INSTALL_PREFIX}/occt-tcl/src)
