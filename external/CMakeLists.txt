#### ################ ####
#### #### OpenCL #### ####
#### ################ ####
find_package(OPENCL)
if (${OPENCL_FOUND})
    message(STATUS "OpenCL found.")
else (${OPENCL_FOUND})
    message(FATAL_ERROR "OpenCL not found.")
endif (${OPENCL_FOUND})

#### ################ ####
#### #### OpenGL #### ####
#### ################ ####
find_package(opengl REQUIRED)

if (${OPENGL_FOUND})
    message(STATUS "OpenGL found.")
else (${OPENGL_FOUND})
    message(FATAL_ERROR "OpenGL not found.")
endif (${OPENGL_FOUND})

#### ################# ####
#### #### NanoGUI #### ####
#### ################# ####
add_subdirectory(nanogui)
set(NANOGUI_EXTRA_INCLUDE_DIRS
        ${CMAKE_CURRENT_SOURCE_DIR}/nanogui/ext/eigen
        ${CMAKE_CURRENT_SOURCE_DIR}/nanogui/ext/glfw/include
        ${CMAKE_CURRENT_SOURCE_DIR}/nanogui/ext/nanovg/src)

#### ############# ####
#### #### GLM #### ####
#### ############# ####
find_package(GLM)
if (${GLM_FOUND})
    message(STATUS "GLM found.")
else (${GLM_FOUND})
    message(FATAL_ERROR "GLM not found.")
endif (${GLM_FOUND})

#### ############## ####
#### #### BWGL #### ####
#### ############## ####
add_subdirectory(bwgl)
message(WARNING ${BWGL_EXTRA_INCLUDE_DIRS})

#### Khronos C++ OpenCL Wrapper API ####

#### ################# ####
#### #### LodePNG #### ####
#### ################# ####
set(LODEPNG_SOURCE_FILES
        ${CMAKE_CURRENT_SOURCE_DIR}/lodepng/lodepng.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/lodepng/lodepng_util.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/lodepng/pngdetail.cpp)
set(LODEPNG_HEADER_FILES
        ${CMAKE_CURRENT_SOURCE_DIR}/lodepng/lodepng.h
        ${CMAKE_CURRENT_SOURCE_DIR}/lodepng/lodepng_util.h)
add_library(lodepng STATIC ${LODEPNG_SOURCE_FILES} ${LODEPNG_HEADER_FILES})

#### ################ ####
#### #### Assimp #### ####
#### ################ ####
find_package(Assimp REQUIRED)

#### Export libraries and header search paths to CMake parent scope ####
set(EXTERNAL_LIBRARIES
        ${OpenCL_LIBRARIES}
        ${OPENGL_LIBRARIES}
        nanogui
        bwgl
        lodepng
        ${ASSIMP_LIBRARY})

set(EXTERNAL_INCLUDE_DIRS
        ${OpenCL_INCLUDE_DIRS}
        ${OPENGL_INCLUDE_DIR}
        ${NanoGUI_SOURCE_DIR}/include
        ${NANOGUI_EXTRA_INCLUDE_DIRS}
        ${GLM_INCLUDE_DIRS}
        ${CMAKE_CURRENT_SOURCE_DIR}/bwgl/include
        ${BWGL_EXTRA_INCLUDE_DIRS}
        ${ASSIMP_INCLUDE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR})

### Print libraries to CMake output ###
message(WARNING "external libraries:")
foreach (EXTERNAL_LIBRARY ${EXTERNAL_LIBRARIES})
    message(WARNING "	${EXTERNAL_LIBRARY}")
endforeach (EXTERNAL_LIBRARY)
message(WARNING "external include directories:")
foreach (EXTERNAL_INCLUDE_DIR ${EXTERNAL_INCLUDE_DIRS})
    message(WARNING "	${EXTERNAL_INCLUDE_DIR}")
endforeach (EXTERNAL_INCLUDE_DIR)

### Export libraries and include dirs to parent scope
set(EXTERNAL_LIBRARIES ${EXTERNAL_LIBRARIES} PARENT_SCOPE)
set(EXTERNAL_INCLUDE_DIRS ${EXTERNAL_INCLUDE_DIRS} PARENT_SCOPE)