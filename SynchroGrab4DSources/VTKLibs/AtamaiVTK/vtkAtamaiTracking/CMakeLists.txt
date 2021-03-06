PROJECT(ATAMAITRACKING)

SET(KIT AtamaiTracking)
SET(UKIT ATAMAITRACKING)
SET(KIT_LIBS vtkHybrid vtkParallel)
SET(KIT_EXTRA_LIBS)

SET ( Kit_SRCS
vtkTracker.cxx
vtkTrackerTool.cxx
vtkFakeTracker.cxx
vtkTrackerBuffer.cxx
vtkFrameToTimeConverter.cxx
)

SET_SOURCE_FILES_PROPERTIES(
vtkTracker
ABSTRACT
)

# vtkAtamaiTracking options
OPTION (VTK_USE_POLARIS "Provide support for the NDI POLARIS and AURORA" ON)
OPTION (VTK_USE_CERTUS "Provide support for the NDI Certus" OFF)
OPTION (VTK_USE_FLOCK "Provide support for the Ascension Flock of Birds" ON)
OPTION (VTK_USE_MICRONTRACKER "Provide support for the Claron MicronTracker" OFF)

IF (VTK_USE_CERTUS)
  SET ( Kit_SRCS ${Kit_SRCS}
	vtkNDICertusTracker.cxx)
  FIND_PATH(NDIOAPI_INCLUDE_DIR ndopto.h)
  FIND_LIBRARY(NDIOAPI_LIBRARY oapi)
  INCLUDE_DIRECTORIES(${VTKNDICAPI_SOURCE_DIR}
                      ${VTKNDICAPI_BINARY_DIR}
                      ${NDIOAPI_INCLUDE_DIR})
  SET (KIT_EXTRA_LIBS ${KIT_EXTRA_LIBS} vtkndicapi ${NDIOAPI_LIBRARY})
ENDIF (VTK_USE_CERTUS)

IF (VTK_USE_POLARIS)
  SET ( Kit_SRCS ${Kit_SRCS}
        vtkPOLARISTracker.cxx
	vtkNDITracker.cxx)
  SET (KIT_EXTRA_LIBS ${KIT_EXTRA_LIBS} vtkndicapi)
  INCLUDE_DIRECTORIES(${VTKNDICAPI_SOURCE_DIR}
                      ${VTKNDICAPI_BINARY_DIR})
ENDIF (VTK_USE_POLARIS)

IF (VTK_USE_FLOCK)
  SET ( Kit_SRCS ${Kit_SRCS}
        vtkFlockTracker.cxx)
  SET (KIT_EXTRA_LIBS ${KIT_EXTRA_LIBS} vtkflock)
  INCLUDE_DIRECTORIES(${VTKFLOCK_SOURCE_DIR}
                      ${VTKFLOCK_BINARY_DIR})
ENDIF (VTK_USE_FLOCK)

IF (VTK_USE_MICRONTRACKER)
  SET ( AtamaiTracking_SRCS ${AtamaiTracking_SRCS}
	vtkMicronTracker.cxx)
  INCLUDE_DIRECTORIES(${VTKMICRONTRACKERFILES_SOURCE_DIR}
                      ${VTKMICRONTRACKERFILES_BINARY_DIR})
  ##TARGET_LINK_LIBRARIES (vtkAtamaiTracking vtkMicronTrackerFiles)
  INCLUDE (${CMAKE_ROOT}/Modules/FindOpenGL.cmake)
ENDIF (VTK_USE_MICRONTRACKER)

SET(Kit_EXTRA_SRCS)
SET(Kit_EXTRA_CMDS)
SET(Kit_TCL_EXTRA_SRCS)
SET(Kit_PYTHON_EXTRA_SRCS)
SET(Kit_JAVA_EXTRA_SRCS)

# -----------------------------------------------------------------

# Include the common setting file
INCLUDE(${SYNCHROGRAB4D_SOURCE_DIR}/CMake/KitCommonBlock.cmake)
