cmake_minimum_required(VERSION 2.8.3)
project(cv_bridge)

find_package(catkin REQUIRED COMPONENTS rosconsole sensor_msgs)
find_package(Boost REQUIRED python3 python)
find_package(OpenCV 3 REQUIRED)
find_package(Python3 COMPONENTS Interpreter Development)

catkin_package(
    INCLUDE_DIRS include
    LIBRARIES ${PROJECT_NAME}
    CATKIN_DEPENDS rosconsole sensor_msgs
    DEPENDS OpenCV
)

catkin_python_setup()

include_directories(include ${Boost_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS})

if(CMAKE_VERSION VERSION_LESS "3.13")
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

# Make sure the compiler can find include files for our cv_bridge library
# when other libraries or executables link to cv_bridge
include_directories(${PROJECT_SOURCE_DIR}/include)

add_library(${PROJECT_NAME} src/cv_bridge.cpp src/rgb_colors.cpp)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES} ${catkin_LIBRARIES})

install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
    DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

# Python Bindings
set(Python_ADDITIONAL_VERSIONS 3.6)
find_package(PythonLibs 3.6)
include_directories(${PYTHON_INCLUDE_DIRS})

# These two lines are key for Python 3
set(PYTHON_VERSION_MAJOR 3)
set(PYTHON_VERSION_MINOR 6)
