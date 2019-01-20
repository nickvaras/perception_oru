cmake_minimum_required(VERSION 2.8.3)
set(CMAKE_BUILD_TYPE Release) 
# set(CMAKE_BUILD_TYPE Debug) 
project(ndt_fuser)

find_package(PCL 1.3 REQUIRED COMPONENTS common)
find_package(Eigen3 REQUIRED )
# find_package(OpenGL REQUIRED )
find_package(catkin REQUIRED COMPONENTS 
  rosconsole
  cmake_modules 
  ndt_registration 
  ndt_map 
  ndt_generic
  laser_geometry 
  sensor_msgs 
  visualization_msgs 
  ndt_visualisation 
  std_srvs
#  message_generation
)

catkin_package(
    DEPENDS EIGEN3 PCL #OpenGL 
    CATKIN_DEPENDS ndt_registration ndt_map laser_geometry sensor_msgs ndt_visualisation std_srvs message_runtime visualization_msgs
    INCLUDE_DIRS include
    LIBRARIES ${PROJECT_NAME} 
)

set (${PROJECT_NAME}_LIB_SRCS
#	    src/ndt_fuser.cpp
	    src/ndt_fuser_hmt.cpp
)

set (${PROJECT_NAME}_LIB_ROS_SRCS
	    src/ndt_fuser_ros_wrappers_src/ndt_fuser_init.cpp
)

include_directories(include
    ${catkin_INCLUDE_DIRS}
    ${EIGEN3_INCLUDE_DIRS}
    ${PCL_INCLUDE_DIRS}
    # ${OPENGL_INCLUDE_DIR}
)


find_package(OpenMP)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")

include_directories(${Boost_INCLUDE_DIRS})

add_library(${PROJECT_NAME} ${${PROJECT_NAME}_LIB_SRCS} ${${PROJECT_NAME}_LIB_ROS_SRCS})
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

add_executable(ndt_fuser_node src/ndt_fuser_node.cpp)
# add_dependencies(ndt_fuser_node ndt_fuser_generate_messages_cpp)

target_link_libraries(ndt_fuser_node ${PROJECT_NAME} ${Boost_LIBRARIES} ${catkin_LIBRARIES})# ${OpenGL_LIBRARIES} )


add_executable(ndt_fuser_logger_node src/ndt_fuser_logger_node.cpp)
add_dependencies(ndt_fuser_logger_node ndt_fuser__generate_messages_cpp)

target_link_libraries(ndt_fuser_logger_node ${PROJECT_NAME} ${Boost_LIBRARIES} ${catkin_LIBRARIES}) # ${OPENGL_LIBRARIES} )


add_executable(init_test src/init_test.cpp)
add_dependencies(init_test ndt_fuser__generate_messages_cpp)

target_link_libraries(init_test ${PROJECT_NAME} ${Boost_LIBRARIES} ${catkin_LIBRARIES}) # ${OPENGL_LIBRARIES} )

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
install(TARGETS ndt_fuser_node ${PROJECT_NAME}
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
       )

install(DIRECTORY include/${PROJECT_NAME}/
	DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
       )
install(FILES
   launch/fuser_2dlaser.launch  
   launch/fuser_hmt_vellodyne32.launch  
   launch/fuser_velodyne32.launch  
   launch/velo32.yaml
   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
install(DIRECTORY maps/
	DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
