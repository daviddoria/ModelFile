cmake_minimum_required(VERSION 2.6)

PROJECT(ModelFile)

#########################
FIND_PACKAGE(VXL REQUIRED)
INCLUDE(${VXL_CMAKE_DIR}/UseVXL.cmake)

#################################
set(Boost_USE_MULTITHREADED ON)

FIND_PACKAGE(Boost 1.37 COMPONENTS program_options)

set(PROJECTS /media/portable/Projects/src/)

INCLUDE_DIRECTORIES(
${VXL_RPL_INCLUDE_DIR}
${VXL_MUL_INCLUDE_DIR}
${PROJECTS}
${Boost_INCLUDE_DIRS}
)

LINK_DIRECTORIES(${LINK_DIRECTORIES} ${Boost_LIBRARY_DIRS})

set(GeometryDir ${PROJECTS}/Geometry/)

FIND_PACKAGE(VTK REQUIRED)
INCLUDE(${VTK_USE_FILE})

add_subdirectory(Tests)

set(Sources
ModelFile.cpp

${GeometryDir}/Angles.cpp 
${GeometryDir}/Color.cpp 
${GeometryDir}/Ray.cpp
${GeometryDir}/Edge.cpp  
${GeometryDir}/Triangle.cpp
${GeometryDir}/OrientedPoint.cpp
${GeometryDir}/Transformation.cpp
${GeometryDir}/Geometry.cpp
${GeometryDir}/Helpers.cpp

${PROJECTS}/VXLHelpers/VXLHelpers.cpp
${PROJECTS}/Tools/Tools.cpp

${PROJECTS}/VTKHelpers/Octree.cpp
${PROJECTS}/KDTree/KDTree.cpp
)

set(Libraries
${VTK_LIBRARIES}
vgl vgl_algo vnl vnl_algo vcl vul  vil
boost_program_options-mt
)

add_library(libModelFile ${Sources} )
target_link_libraries(libModelFile ${Libraries})

##################################################
ADD_EXECUTABLE( Converter Converter.cpp)
TARGET_LINK_LIBRARIES(Converter ${Libraries} libModelFile)

################################################
ADD_EXECUTABLE(Combine Combine.cpp)
TARGET_LINK_LIBRARIES(Combine ${Libraries} libModelFile)

################################################
# 
# ADD_EXECUTABLE(ExtractPoints ExtractPoints.cpp)
# TARGET_LINK_LIBRARIES(ExtractPoints ${Libraries})

################################################
# 
# ADD_EXECUTABLE(CleanupModel CleanupModel.cpp)
# TARGET_LINK_LIBRARIES(CleanupModel ${Libraries})

##################################################
ADD_EXECUTABLE(TransformModel TransformModel.cpp)
TARGET_LINK_LIBRARIES(TransformModel ${Libraries} libModelFile)

##################################################
ADD_EXECUTABLE(AddNormals AddNormals.cpp)
TARGET_LINK_LIBRARIES(AddNormals ${Libraries} libModelFile)

##################################################
ADD_EXECUTABLE(CenterModel CenterModel.cpp)
TARGET_LINK_LIBRARIES(CenterModel ${Libraries} libModelFile)

##################################################
ADD_EXECUTABLE(CenterOfMass CenterOfMass.cpp)
TARGET_LINK_LIBRARIES(CenterOfMass ${Libraries} libModelFile)


###################################################
ADD_EXECUTABLE(TriangulateScan TriangulateScan.cpp Triangulate.cpp)
TARGET_LINK_LIBRARIES(TriangulateScan ${Libraries} libModelFile)

###################################################
ADD_EXECUTABLE(RemoveTriangles RemoveTriangles.cpp)
TARGET_LINK_LIBRARIES(RemoveTriangles ${Libraries} libModelFile)

###################################################
ADD_EXECUTABLE(RemoveVertices RemoveVertices.cpp)
TARGET_LINK_LIBRARIES(RemoveVertices ${Libraries} libModelFile)

###################################################
ADD_EXECUTABLE(TransferTriangles TransferTriangles.cpp)
TARGET_LINK_LIBRARIES(TransferTriangles ${Libraries} libModelFile)

###################################################
ADD_EXECUTABLE(AddScannerPosition AddScannerPosition.cpp)
TARGET_LINK_LIBRARIES(AddScannerPosition ${Libraries} libModelFile)

###################################################
ADD_EXECUTABLE(NumPointsInBox NumPointsInBox.cpp)
TARGET_LINK_LIBRARIES(NumPointsInBox ${Libraries} libModelFile)


###################################################
# 
# ADD_EXECUTABLE(TransformAllCarPositions TransformAllCarPositions.cpp)
# TARGET_LINK_LIBRARIES(TransformAllCarPositions ${Libraries})
