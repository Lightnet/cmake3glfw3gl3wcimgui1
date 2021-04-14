# cimgui

cmake_minimum_required(VERSION 3.20) # FetchContent is available in 3.11+

project(cimguiapp VERSION 0.0.1)

#target_compile_definitions(cimguiapp PRIVATE CIMGUI_DEFINE_ENUMS_AND_STRUCTS=1)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(CPP_INCLUDE_DIRS "")
set(CPP_SOURCES_FILES "")
set(CPP_HEADERS_FILES "")

if (NOT glfw_FOUND)
  include(FetchContent)
  FetchContent_Declare(
    glfw
    GIT_REPOSITORY https://github.com/glfw/glfw.git
    GIT_TAG 3.3.4
  )
  FetchContent_GetProperties(glfw)
  if (NOT glfw_POPULATED) # Have we downloaded glfw yet?
    set(FETCHCONTENT_QUIET NO)
    FetchContent_Populate(glfw)

    set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE) # don't build the supplied examples
    set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE) # don't build the tests
    set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE) # don't build the docs
    set(GLFW_INSTALL OFF CACHE INTERNAL "" FORCE)

    #include_directories(${glfw_SOURCE_DIR}/include)
    list(APPEND CPP_INCLUDE_DIRS ${glfw_SOURCE_DIR}/include)

    message("glfw_SOURCE_DIR " ${glfw_SOURCE_DIR})
    message("glfw_BINARY_DIR " ${glfw_BINARY_DIR})

    add_subdirectory(${glfw_SOURCE_DIR} ${glfw_BINARY_DIR})
    link_directories(${glfw_BINARY_DIR}/src/Debug)

  endif()
endif()

if (NOT gl3wgit_FOUND)
  include(FetchContent)
  FetchContent_Declare(
    gl3wgit
    GIT_REPOSITORY https://github.com/skaslev/gl3w.git
    GIT_TAG master
  )
  FetchContent_GetProperties(gl3wgit)
  if (NOT gl3wgit_POPULATED) # Have we downloaded gl3w yet?
    set(FETCHCONTENT_QUIET NO)
    FetchContent_Populate(gl3wgit)
    
    #include_directories(${gl3wgit_BINARY_DIR}/include)
    list(APPEND CPP_INCLUDE_DIRS ${gl3wgit_BINARY_DIR}/include)

    list(APPEND CPP_SOURCES_FILES ${gl3wgit_BINARY_DIR}/src/gl3w.c)

    #add_subdirectory(${glfw_SOURCE_DIR})
    message("gl3wgit_SOURCE_DIR " ${gl3wgit_SOURCE_DIR})
    message("gl3wgit_BINARY_DIR " ${gl3wgit_BINARY_DIR})
    add_subdirectory(${gl3wgit_SOURCE_DIR} ${gl3wgit_BINARY_DIR})

  endif()
endif()

if (NOT cimguigit_FOUND)
  include(FetchContent)
  FetchContent_Declare(
    cimguigit
    GIT_REPOSITORY https://github.com/cimgui/cimgui.git
    GIT_TAG 1.82
    #GIT_TAG master
    
  )
  FetchContent_GetProperties(cimguigit)
  if (NOT cimguigit_POPULATED) # Have we downloaded glfw yet?
    set(FETCHCONTENT_QUIET NO)
    FetchContent_Populate(cimguigit)
    #message("cimguigit_SOURCE_DIR " ${cimguigit_SOURCE_DIR})
    
    #include_directories(${cimguigit_SOURCE_DIR})
    #include_directories(${cimguigit_SOURCE_DIR}/imgui)
    #include_directories(${cimguigit_SOURCE_DIR}/imgui/backends)

    list(APPEND CPP_SOURCES_FILES ${cimguigit_SOURCE_DIR}/cimgui.cpp)
    list(APPEND CPP_HEADERS_FILES ${cimguigit_SOURCE_DIR}/imgui/imgui.cpp)
    list(APPEND CPP_SOURCES_FILES ${cimguigit_SOURCE_DIR}/imgui/imgui_demo.cpp)
    list(APPEND CPP_SOURCES_FILES ${cimguigit_SOURCE_DIR}/imgui/imgui_draw.cpp)
    list(APPEND CPP_SOURCES_FILES ${cimguigit_SOURCE_DIR}/imgui/imgui_tables.cpp)
    list(APPEND CPP_SOURCES_FILES ${cimguigit_SOURCE_DIR}/imgui/imgui_widgets.cpp)
    list(APPEND CPP_SOURCES_FILES ${cimguigit_SOURCE_DIR}/imgui/backends/imgui_impl_glfw.cpp)
    list(APPEND CPP_SOURCES_FILES ${cimguigit_SOURCE_DIR}/imgui/backends/imgui_impl_opengl3.cpp)

    list(APPEND CPP_HEADERS_FILES ${cimguigit_SOURCE_DIR}/cimgui.h)
    list(APPEND CPP_HEADERS_FILES ${cimguigit_SOURCE_DIR}/imgui/imgui.h)
    list(APPEND CPP_HEADERS_FILES ${cimguigit_SOURCE_DIR}/imgui/imconfig.h)
    list(APPEND CPP_HEADERS_FILES ${cimguigit_SOURCE_DIR}/imgui/imgui_internal.h)
    #list(APPEND CPP_HEADERS_FILES ${cimguigit_SOURCE_DIR}/imgui/imstb_rectpack.h)
    #list(APPEND CPP_HEADERS_FILES ${cimguigit_SOURCE_DIR}/imgui/imstb_textedit.h)
    #list(APPEND CPP_HEADERS_FILES ${cimguigit_SOURCE_DIR}/imgui/imstb_truetype.h)
    list(APPEND CPP_HEADERS_FILES ${cimguigit_SOURCE_DIR}/imgui/backends/imgui_impl_glfw.h)
    list(APPEND CPP_HEADERS_FILES ${cimguigit_SOURCE_DIR}/imgui/backends/imgui_impl_opengl3.h)

    #list(APPEND CPP_INCLUDE_DIRS ${cimguigit_SOURCE_DIR})
    list(APPEND CPP_INCLUDE_DIRS ${cimguigit_SOURCE_DIR}/imgui)
    list(APPEND CPP_INCLUDE_DIRS ${cimguigit_SOURCE_DIR}/imgui/backends)
    
    add_subdirectory(${cimguigit_SOURCE_DIR} ${cimguigit_BINARY_DIR})

    link_directories(${cimguigit_SOURCE_DIR}/build/Debug)
    
  endif()
endif()

#look for lib file
#find_library(glfw3_LIB NAMES glfw3 HINTS ${glfw_BINARY_DIR}/src/Debug) #lib search
#find_library(cimgui_LIB NAMES cimgui HINTS ${cimguigit_BINARY_DIR}/Debug) #lib search
#find_library(cimgui_LIB NAMES cimgui HINTS ${cimguigit_SOURCE_DIR}/build/Debug) #lib search

include_directories(${CPP_INCLUDE_DIRS}) #need to be added in else error

add_executable(${PROJECT_NAME}
  #cimguitest.c #fail
  #glfw3gl3w.cpp
  glfw3gl3wcimgui1.c
  ${CPP_HEADERS_FILES}
  ${CPP_SOURCES_FILES}
)

target_link_libraries(${PROJECT_NAME} 
  PRIVATE 
  glfw3
  cimgui
  #${glfw3_LIB} 
  #${cimgui_LIB} 
  ${CMAKE_DL_LIBS}
)


