
cmake_minimum_required(VERSION 3.3 FATAL_ERROR)

project(Polyhedron)

find_package(VTK COMPONENTS 
  vtkCommonColor
  vtkCommonCore
  vtkCommonDataModel
  vtkIOXML
  vtkInteractionStyle
  vtkRenderingCore
  vtkRenderingFreeType
  vtkRenderingOpenGL2 QUIET)
if (NOT VTK_FOUND)
  message("Skipping Polyhedron: ${VTK_NOT_FOUND_MESSAGE}")
  return ()
endif()
message (STATUS "VTK_VERSION: ${VTK_VERSION}")
if (VTK_VERSION VERSION_LESS "8.90.0")
  # old system
  include(${VTK_USE_FILE})
  add_executable(Polyhedron MACOSX_BUNDLE Polyhedron.cxx )
  target_link_libraries(Polyhedron PRIVATE ${VTK_LIBRARIES})
else ()
  # include all components
  add_executable(Polyhedron MACOSX_BUNDLE Polyhedron.cxx )
  target_link_libraries(Polyhedron PRIVATE ${VTK_LIBRARIES})
  # vtk_module_autoinit is needed
  vtk_module_autoinit(
    TARGETS Polyhedron
    MODULES ${VTK_LIBRARIES}
    )
endif () 
