set(GRABBERS_SRC grabber.cpp)

if(WITH_OPENNI2)
  find_package(OpenNI2)
  if(OPENNI2_FOUND)
    list(APPEND GRABBERS_SRC openni2_grabber.cpp)
    list(APPEND GRABBERS_DEPS openni2)
  endif()
endif()

if(WITH_REALSENSE)
  find_package(RealSense 1.9.7)
  if(REALSENSE_FOUND)
    list(APPEND GRABBERS_SRC realsense_grabber.cpp)
    list(APPEND GRABBERS_DEPS realsense)
  endif()
endif()

if(WITH_PYLON)
  find_package(Pylon)
  if(PYLON_FOUND)
    list(APPEND GRABBERS_SRC pylon_grabber.cpp)
    list(APPEND GRABBERS_DEPS ${PYLON_LIBRARIES})
  endif()
endif()

add_library(grabbers ${GRABBERS_SRC})
target_include_directories(grabbers PUBLIC ${Boost_INCLUDE_DIRS})
target_link_libraries(grabbers opencv_core ${GRABBERS_DEPS})

foreach(grabber openni2 realsense pylon)
  string(TOUPPER ${grabber} GRABBER)
  if(${GRABBER}_FOUND)
    target_compile_definitions(grabbers PRIVATE "-DHAVE_${GRABBER}")
  endif()
endforeach()
