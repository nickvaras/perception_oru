cmake_minimum_required(VERSION 2.8.3)
project(ndt_feature_finder)

add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
	ndt_map
	ndt_fuser
	rviz)

find_package(OpenCV 3.2.0 REQUIRED)
find_package(PCL REQUIRED)
find_package(Eigen3 REQUIRED)
find_package( Boost REQUIRED )

catkin_package(
 INCLUDE_DIRS include
 LIBRARIES ndt_corner
 CATKIN_DEPENDS ndt_map ndt_fuser rviz
 DEPENDS eigen3 boost pcl opencv
)

include_directories(include ${catkin_INCLUDE_DIRS} ${EIGEN_INCLUDE_DIRS} ${PCL_INCLUDE_DIRS})
add_definitions(${EIGEN_DEFINITIONS})

## Declare a C++ library
# add_library(ndt_feature
#   src/${PROJECT_NAME}/ndt_feature.cpp
# )

add_library(ndt_corner src/ndt_corner.cpp)

## Add cmake target dependencies of the library
## as an example, code may need to be generated before libraries
## either from message generation or dynamic reconfigure
# add_dependencies(ndt_feature ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Declare a C++ executable
# add_executable(ndt_feature_node src/ndt_feature_node.cpp)

## Add cmake target dependencies of the executable
## same as for the library above
# add_dependencies(ndt_feature_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
# target_link_libraries(ndt_feature_node
#   ${catkin_LIBRARIES}
# )

add_subdirectory(tests)
