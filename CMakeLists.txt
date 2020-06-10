
cmake_minimum_required(VERSION 2.8)
project(OpenGLHelloWorld)

# Parameters
set(DO_YOU_WANT_TO_EXPORT_CMAKE_COMPILE_COMMANDS FALSE CACHE BOOL "Do you want to force to export cmake compile commands?")

if(DO_YOU_WANT_TO_EXPORT_CMAKE_COMPILE_COMMANDS)
    set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
    message(STATUS "Exporting compile_commands.json")
else()
    set(CMAKE_EXPORT_COMPILE_COMMANDS OFF)
endif()

IF( EXISTS "${CMAKE_CURRENT_BINARY_DIR}/compile_commands.json" )
  EXECUTE_PROCESS( COMMAND ${CMAKE_COMMAND} -E copy_if_different
    ${CMAKE_CURRENT_BINARY_DIR}/compile_commands.json
    ${CMAKE_CURRENT_SOURCE_DIR}/compile_commands.json
  )
ENDIF()

# Compile properties
if(CMAKE_BUILD_TYPE STREQUAL "Build")
    add_compile_options(-g)
endif()

add_compile_options(-std=c++17)
add_compile_options(-O2)

# Linking settings
if(UNIX)
    link_directories(${PROJECT_SOURCE_DIR}/extlib/linux/shared)
    link_directories(${PROJECT_SOURCE_DIR}/extlib/linux/static)
elseif(WIN32)
# To be added in the nearest future
#   link_directories(${PROJECT_SOURCE_DIR}/extlib/windows/shared)
#   link_directories(${PROJECT_SOURCE_DIR}/extlib/windows/static)
endif()

# Include settings
if (UNIX)
    include_directories(${PROJECT_SOURCE_DIR}/impl/src/
    ${PROJECT_SOURCE_DIR}/impl/include
    )
endif()

# Compile directories
add_subdirectory(${PROJECT_SOURCE_DIR}/impl)

# Executables settings

message(STATUS "Building OpenGLHelloWorld")
add_executable(glhello
    ${SOURCE}
    ${HEADERS}
    )
set_target_properties(glhello PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(glhello
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
    )



# Targget Linking settings
if(UNIX)
        target_link_libraries(glhello)
endif()
