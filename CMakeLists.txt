# ----------------------------------------------------------------------------
#  CMake file for libsvm. See root CMakeLists.txt
#
# ----------------------------------------------------------------------------

project(libsvm)

set(CURR_INCLUDE_DIR "${CMAKE_CURRENT_LIST_DIR}")

set_property(GLOBAL PROPERTY LIBSVM_INCLUDE_DIR ${CURR_INCLUDE_DIR})
ocv_include_directories(${CURR_INCLUDE_DIR})

file(GLOB_RECURSE svm_headers RELATIVE "${CMAKE_CURRENT_LIST_DIR}" "svm.h")
#file(GLOB_RECURSE svm_sources RELATIVE "${CMAKE_CURRENT_LIST_DIR}" "*.c")
file(GLOB_RECURSE svm_sources RELATIVE "${CMAKE_CURRENT_LIST_DIR}" "svm.cpp")

message(STATUS "libsvm will be used")

# ----------------------------------------------------------------------------------
#         Define the library target:
# ----------------------------------------------------------------------------------


#add_library(${PROJECT_NAME} STATIC ${OPENCV_3RDPARTY_EXCLUDE_FROM_ALL} ${svm_headers} ${svm_sources} ${svmcpp_sources})
add_library(${PROJECT_NAME} STATIC ${OPENCV_3RDPARTY_EXCLUDE_FROM_ALL} ${svm_headers} ${svm_sources})
ocv_warnings_disable(CMAKE_C_FLAGS -Wunused-variable)

set_target_properties(${PROJECT_NAME} PROPERTIES
  OUTPUT_NAME ${PROJECT_NAME}
  DEBUG_POSTFIX "${OPENCV_DEBUG_POSTFIX}"
  COMPILE_PDB_NAME ${PROJECT_NAME}
  COMPILE_PDB_NAME_DEBUG "${PROJECT_NAME}${OPENCV_DEBUG_POSTFIX}"
  ARCHIVE_OUTPUT_DIRECTORY ${3P_LIBRARY_OUTPUT_PATH}
  )


target_compile_definitions(${PROJECT_NAME} PUBLIC SPNG_STATIC)

if(ENABLE_SOLUTION_FOLDERS)
    set_target_properties(${PROJECT_NAME} PROPERTIES FOLDER "3rdparty")
endif()

if(NOT BUILD_SHARED_LIBS)
    ocv_install_target(${PROJECT_NAME} EXPORT OpenCVModules ARCHIVE DESTINATION ${OPENCV_3P_LIB_INSTALL_PATH} COMPONENT dev OPTIONAL)
endif()

#ocv_install_3rdparty_licenses(${PROJECT_NAME} LICENSE)
