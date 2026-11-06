cmake_minimum_required(VERSION 3.8)
project(keep_going_arcanain)

find_package(ament_cmake REQUIRED)

# Install launch files once the launch directory is added.
if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/launch")
  install(DIRECTORY launch
    DESTINATION share/${PROJECT_NAME}
  )
endif()

install(FILES README.md LICENSE
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
