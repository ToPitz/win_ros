cmake_minimum_required(VERSION 2.8.3)
project(win_roscpp_tutorials)

find_package(Boost REQUIRED COMPONENTS date_time thread)
find_package(catkin REQUIRED COMPONENTS message_generation rostime roscpp rosconsole roscpp_serialization)

include_directories(${catkin_INCLUDE_DIRS})
link_directories(${catkin_LIBRARY_DIRS})

add_service_files(DIRECTORY srv FILES TwoInts.srv)
generate_messages(DEPENDENCIES std_msgs)

catkin_package(CATKIN_DEPENDS message_runtime std_msgs)

macro(rostutorial T)
  add_executable(win_${T} ${T}/${T}.cpp)
  target_link_libraries(win_${T} ${catkin_LIBRARIES} ${Boost_LIBRARIES})
  add_dependencies(win_${T} win_roscpp_tutorials_gencpp)
  install(TARGETS win_${T} RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
endmacro()

foreach(dir
    listener
    notify_connect
    talker
    babbler
    add_two_ints_client
    add_two_ints_server
    add_two_ints_server_class
    anonymous_listener
    listener_with_userdata
    listener_multiple
    listener_threaded_spin
    listener_async_spin
    # listener_single_message
    listener_with_tracked_object
    listener_unreliable
    listener_class
    node_handle_namespaces
    custom_callback_processing
    timers
    parameters
    )
  rostutorial(${dir})
endforeach()

add_executable(win_time_api_sleep time_api/sleep/sleep.cpp)
target_link_libraries(win_time_api_sleep ${catkin_LIBRARIES})
install(TARGETS win_time_api_sleep RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
