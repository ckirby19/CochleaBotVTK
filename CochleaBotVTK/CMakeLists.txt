
cmake_minimum_required(VERSION 3.3 FATAL_ERROR)

set(PROJECT_NAME
    CochleaBotVTK
)

project(${PROJECT_NAME})

find_package(VTK COMPONENTS
  vtkColor
  vtkCommonCore
  vtkFiltersSources
  vtkInteractionStyle
  vtkRenderingContextOpenGL2
  vtkRenderingCore
  vtkRenderingFreeType
  vtkRenderingGL2PSOpenGL2
  vtkRenderingOpenGL2 QUIET
)

if (NOT VTK_FOUND)
  message("Skipping CylinderExample: ${VTK_NOT_FOUND_MESSAGE}")
  return ()
endif()
message (STATUS "VTK_VERSION: ${VTK_VERSION}")



set(SOURCES
   main.cpp
)

set(HEADERS
)

set(VTKC /usr)
link_directories(${VTKC}/lib/vtk-5.8) #Contains all the .cmake files (UseVTK.cmake etc)
include_directories(${VTKC}/include/vtk-5.8) #Contains all the .h files (vtk3Ds.h etc)


if (VTK_VERSION VERSION_LESS "8.90.0")
  # old system
  include(${VTK_USE_FILE})
  add_executable(${PROJECT_NAME} MACOSX_BUNDLE main.cpp )
  target_link_libraries(${PROJECT_NAME} PRIVATE ${VTK_LIBRARIES})
else ()
  # include all components
  add_executable(${PROJECT_NAME} MACOSX_BUNDLE main.cpp )
  target_link_libraries(${PROJECT_NAME} PRIVATE ${VTK_LIBRARIES})
  # vtk_module_autoinit is needed
  vtk_module_autoinit(
    TARGETS ${PROJECT_NAME}
    MODULES ${VTK_LIBRARIES}
    )
endif ()
