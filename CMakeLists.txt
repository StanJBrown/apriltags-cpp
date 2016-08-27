CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

PROJECT(swatbotics_april_tags)

SET(CMAKE_CXX_FLAGS "-Wall -g")
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wsign-compare -frounding-math")
SET(CMAKE_CXX_FLAGS_DEBUG "-O")
SET(CMAKE_CXX_FLAGS_RELEASE "-O2")


FIND_PACKAGE( CGAL QUIET COMPONENTS  )
IF (CGAL_FOUND)
    INCLUDE( ${CGAL_USE_FILE} )
    ADD_DEFINITIONS(-DHAVE_CGAL)
    FIND_PACKAGE( Boost REQUIRED )
ELSE()
    MESSAGE("CGAL not found, can not use new quad detector")
ENDIF()


FIND_PACKAGE(OpenCV REQUIRED)
INCLUDE_DIRECTORIES(
    include/
    ${OpenCV_INCLUDE_DIRS}
    ${catkin_INCLUDE_DIRS}
)

FILE(GLOB_RECURSE apriltags_swatbotics_headers "include/*.h")

ADD_LIBRARY(
    ${PROJECT_NAME}
    src/CameraUtil.cpp
    src/DebugImage.cpp
    src/Geometry.cpp
    src/GrayModel.cpp
    src/MathUtil.cpp
    src/Refine.cpp
    src/TagDetector.cpp
    src/TagFamily.cpp
    src/TagFamilies.cpp
    src/UnionFindSimple.cpp
)

TARGET_LINK_LIBRARIES(
    ${PROJECT_NAME}
    ${OpenCV_LIBRARIES}
)

INSTALL(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION lib
)

INSTALL(DIRECTORY include/ DESTINATION include/swatbotics_april_tags
    FILES_MATCHING PATTERN "*h"
)
