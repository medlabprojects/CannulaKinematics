CMAKE_MINIMUM_REQUIRED( VERSION 2.8 )
PROJECT( CannulaKinematics )

set(CANNULAKINEMATICS_MAJOR_VERSION 0)
set(CANNULAKINEMATICS_MINOR_VERSION 1)
set(CANNULAKINEMATICS_PATCH_VERSION 0)
set(CANNULAKINEMATICS_VERSION
  ${CANNULAKINEMATICS_MAJOR_VERSION}.${CANNULAKINEMATICS_MINOR_VERSION}.${CANNULAKINEMATICS_PATCH_VERSION})
  
set(INSTALL_LIB_DIR lib CACHE PATH "Installation directory for libraries")
set(INSTALL_BIN_DIR bin CACHE PATH "Installation directory for executables")
set(INSTALL_INCLUDE_DIR include CACHE PATH "Installation directory for header files")

set(INSTALL_CMAKE_DIR	lib/cmake/CannulaKinematics CACHE PATH "Installation directory for cmake files")

# Make relative paths absolute (needed later on)
foreach(p LIB BIN INCLUDE CMAKE)
  set(var INSTALL_${p}_DIR)
  if(NOT IS_ABSOLUTE "${${var}}")
    set(${var} "${CMAKE_INSTALL_PREFIX}/${${var}}")
  endif()
endforeach()


SET( CMAKE_DEBUG_POSTFIX "d" )
SET( SRCS	KinematicOptions.cpp
		Kinematics.cpp
		cannula.cpp
		tube.cpp
		straightcurvedtube.cpp )
SET( HDRS	KinematicOptions.h
		Kinematics.h
		cannula.h
		tube.h
		straightcurvedtube.h )
		
SET( project_SRCS ${SRCS} ${HDRS} )

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)
# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)

find_package(Qt5Xml	REQUIRED)

SET(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")

FIND_PACKAGE(Eigen3	REQUIRED)
INCLUDE_DIRECTORIES(${EIGEN3_INCLUDE_DIR})

FIND_PACKAGE(MathTools)
INCLUDE_DIRECTORIES(${MathTools_INCLUDE_DIRS})

ADD_LIBRARY( CannulaKinematics	STATIC	${project_SRCS} )
SET_TARGET_PROPERTIES( CannulaKinematics PROPERTIES PUBLIC_HEADER	"${HDRS}")
TARGET_LINK_LIBRARIES( CannulaKinematics Qt5::Xml )
INSTALL( TARGETS CannulaKinematics
	 EXPORT CannulaKinematicsTargets
	 RUNTIME DESTINATION ${INSTALL_BIN_DIR} OPTIONAL
	 LIBRARY DESTINATION ${INSTALL_LIB_DIR}
	 ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
	 PUBLIC_HEADER DESTINATION ${INSTALL_INCLUDE_DIR})
	 
# Add all targets to the build-tree export set
EXPORT(TARGETS CannulaKinematics
  FILE "${PROJECT_BINARY_DIR}/CannulaKinematicsTargets.cmake")
 
# Export the package for use from the build-tree
# (this registers the build-tree with a global CMake-registry)
EXPORT(PACKAGE CannulaKinematics)

set(CONF_INCLUDE_DIRS "${INSTALL_INCLUDE_DIR}")
set(CONF_TARGET_LIBRARY_DEBUG	"${INSTALL_LIB_DIR}/CannulaKinematicsd")
set(CONF_TARGET_LIBRARY_RELEASE "${INSTALL_LIB_DIR}/CannulaKinematics")
configure_file(CannulaKinematicsConfig.cmake.in
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/CannulaKinematicsConfig.cmake" @ONLY)
configure_file(CannulaKinematicsConfigVersion.cmake.in
  "${PROJECT_BINARY_DIR}/CannulaKinematicsConfigVersion.cmake" @ONLY)
  
install(FILES
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/CannulaKinematicsConfig.cmake"
  "${PROJECT_BINARY_DIR}/CannulaKinematicsConfigVersion.cmake"
  DESTINATION "${INSTALL_CMAKE_DIR}" COMPONENT dev)
  
install(EXPORT CannulaKinematicsTargets DESTINATION
	"${INSTALL_CMAKE_DIR}" COMPONENT dev)