cmake_minimum_required(VERSION 3.10)
project(MAgneticHaptic_Project)
find_package( OpenCV REQUIRED )
find_package (Threads REQUIRED)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -D_LINUX -no-pie -lm")
link_directories("/home/ashkan/sensoray/driver")
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
add_definitions(-DOSTYPE_linux)

add_library(
	TattileCamera
	TattileCamera.cpp
	TattileCamera.hpp
	)

add_library(
	PoseEstimate
	PoseEstimate.cpp
	PoseEstimate.hpp
	)

add_library(
	Filter
	Filter.cpp
	Filter.hpp
	)

add_library(
	Simple826
	Simple826.cpp
	Simple826.hpp
	)


add_library(
	GPIO826
	GPIO826.cpp
	GPIO826.hpp
	)


add_library(
	Controller
	Controller.cpp
	Controller.hpp
	)



add_executable(u3_demo main.cpp)

INCLUDE_DIRECTORIES("/home/ashkan/sensoray/driver")
INCLUDE_DIRECTORIES("/usr/include/eigen3", /usr/local/lib)


target_link_libraries(PoseEstimate PRIVATE  ${OpenCV_LIBS} ${CMAKE_THREAD_LIBS_INIT} m)
target_link_libraries(TattileCamera PRIVATE  ${OpenCV_LIBS} ${CMAKE_THREAD_LIBS_INIT} m)
target_link_libraries(Simple826 PRIVATE  "/home/ashkan/sensoray/demo/lib826_64.a" m)
target_link_libraries(GPIO826 PRIVATE  Simple826 "/home/ashkan/sensoray/demo/lib826_64.a" m)
target_link_libraries(Controller PRIVATE "/home/ashkan/eigen-3.3.9/Eigen" m)
target_link_libraries(Filter PRIVATE  ${OpenCV_LIBS} ${CMAKE_THREAD_LIBS_INIT} m)




target_link_libraries(u3_demo PRIVATE GPIO826 Simple826 Controller Filter PoseEstimate TattileCamera  ${OpenCV_LIBS} "/home/ashkan/sensoray/demo/lib826_64.a" m)









