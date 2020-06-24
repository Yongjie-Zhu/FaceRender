cmake_minimum_required(VERSION 3.9)

project(FaceRendererHM)
#generate compile_commands.json file
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)
get_filename_component(ROOT_DIR ${PROJECT_SOURCE_DIR} DIRECTORY)

# windows settings
# find_package(glad CONFIG REQUIRED)
# find_package(glfw3 CONFIG REQUIRED)
# find_package(glm REQUIRED)
# ubuntu settings
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

# OpenGL
find_package(OpenGL REQUIRED)

# GLM
find_package(GLM REQUIRED)
message(STATUS "GLM included at ${GLM_INCLUDE_DIR}")
add_library(glm INTERFACE IMPORTED)
set_target_properties(glm PROPERTIES
  INTERFACE_INCLUDE_DIRECTORIES "${GLM_INCLUDE_DIR}"
)

# GLFW
find_package(GLFW3 REQUIRED)
message(STATUS "Found GLFW3 in ${GLFW3_INCLUDE_DIR}")

# glad
set(GLAD_DIR "${CMAKE_SOURCE_DIR}/mythirds/glad")
add_library("glad" "${GLAD_DIR}/src/glad.c")
target_include_directories("glad" PRIVATE "${GLAD_DIR}/include")

# doxgen
include(UseDoxygenDoc)
add_doxygen_doc(
  BUILD_DIR
    ${CMAKE_CURRENT_BINARY_DIR}/_build
  DOXY_FILE
    ${CMAKE_CURRENT_SOURCE_DIR}/docs/Doxyfile.in
  TARGET_NAME
    docs
  COMMENT
    "HTML documentation"
  )

include_directories(includes)
include_directories(${PROJECT_SOURCE_DIR})

file(GLOB_RECURSE FaceRenderer_Headers 
    "depvars.h" 
    "includes/*.h" 
    "includes/*.cc"
    "includes/*.cpp"
)

add_subdirectory(renderer_hm)


