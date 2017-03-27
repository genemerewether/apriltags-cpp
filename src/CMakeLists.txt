add_library(apriltagscpp
  CameraUtil.cpp
  DebugImage.cpp
  Geometry.cpp
  GrayModel.cpp
  MathUtil.cpp
  Refine.cpp
  TagDetector.cpp
  TagFamily.cpp
  TagFamilies.cpp
  UnionFindSimple.cpp
)

if (APPLE)
   string(REPLACE "-llibtbb.dylib" "-ltbb" OPENCV_LDFLAGS "${OPENCV_LDFLAGS}")
endif()

target_link_libraries(apriltagscpp ${OPENCV_LDFLAGS})

set(AT_LIBS ${OPENCV_LDFLAGS} apriltagscpp)

add_executable(camtest camtest.cpp)
target_link_libraries(camtest ${AT_LIBS})

add_executable(tagtest tagtest.cpp)
target_link_libraries(tagtest ${AT_LIBS})

add_executable(quadtest quadtest.cpp)
target_link_libraries(quadtest ${AT_LIBS})

if (GLUT_LIBRARY)
  add_executable(gltest gltest.cpp)
  target_link_libraries(gltest ${GLUT_LIBRARY} ${OPENGL_LIBRARY} ${AT_LIBS})
endif()

if (CAIRO_FOUND)
  add_executable(maketags maketags.cpp)
  target_link_libraries(maketags ${CAIRO_LIBRARIES} ${AT_LIBS} ${CAIRO_LIBS})
endif() 

set_target_properties(apriltagscpp PROPERTIES
  PUBLIC_HEADER "AprilTypes.h;CameraUtil.h;DebugImage.h;Geometry.h;GrayModel.h;MathUtil.h;Profiler.h;Refine.h;TagDetection.h;TagDetector.h;TagFamily.h;UnionFindSimple.h")

install(TARGETS maketags apriltagscpp
  EXPORT apriltagscppTargets
  RUNTIME DESTINATION "${INSTALL_BIN_DIR}"
  LIBRARY DESTINATION "${INSTALL_LIB_DIR}"
  ARCHIVE DESTINATION "${INSTALL_LIB_DIR}"
  PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}")
