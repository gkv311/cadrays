cmake_minimum_required (VERSION 3.13)

project (ImportExport)

set (USED_SOURCES
  AisMesh.cxx
  DataContext.cxx
  DataModel.cxx
  DataNode.cxx
  ImportExport.cxx
  ImportExportPlugin.cxx
  MeshImporter.cxx
  TextureManager.cxx
  Utils.cxx
)
set (USED_HEADERS
  AisMesh.hxx
  DataContext.hxx
  DataModel.hxx
  DataNode.hxx
  ImportExport.hxx
  ImportExportPlugin.hxx
  MeshImporter.hxx
  TextureManager.hxx
  Utils.hxx
)
source_group ("Source Files" FILES ${USED_SOURCES})
source_group ("Header Files" FILES ${USED_HEADERS})

add_library (${PROJECT_NAME} SHARED ${USED_SOURCES} ${USED_HEADERS})
set_target_properties (${PROJECT_NAME} PROPERTIES INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}")

set (anOcctLibs TKViewerTest TKDraw TKV3d TKService TKPrim TKTopAlgo TKGeomAlgo TKBRep TKGeomBase TKG3d TKG2d TKMath TKernel)
target_link_libraries (${PROJECT_NAME} PRIVATE ${anOcctLibs})
target_link_libraries (${PROJECT_NAME} PRIVATE assimp::assimp)

install (TARGETS ${PROJECT_NAME} RUNTIME DESTINATION "${CMAKE_INSTALL_PREFIX}")
