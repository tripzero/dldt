# Copyright (C) 2018-2019 Intel Corporation
# SPDX-License-Identifier: Apache-2.0
#

####################################
## All next project will use C++11
set (CMAKE_CXX_STANDARD 11)
set (CMAKE_CXX_STANDARD_REQUIRED ON)
####################################

if(ENABLE_MKL_DNN)
    add_subdirectory(mkldnn_plugin)
endif()

if(ENABLE_CLDNN)
    add_subdirectory(cldnn_engine)
endif()

if(ENABLE_VPU)
  add_subdirectory(vpu)
endif()

if (ENABLE_GNA)
    add_subdirectory(gna_plugin)
endif()

add_subdirectory(inference_engine)

add_subdirectory(hetero_plugin)

set(InferenceEngine_LIBRARIES inference_engine)
set(InferenceEngine_INCLUDE_DIRS ${CMAKE_SOURCE_DIR}/include)
set(InferenceEngine_SRC_DIRS ${CMAKE_SOURCE_DIR}/src)

# Fake set_target_cpu_flags() function. Currently it is used only for sample build.
# When IE will provide architecture optimized binary for each platform it will be switched on.
function(set_target_cpu_flags TARGET_NAME)
endfunction()

add_subdirectory(extension EXCLUDE_FROM_ALL)
add_library(IE::ie_cpu_extension ALIAS ie_cpu_extension)

file(GLOB_RECURSE EXTENSION_SOURCES extension/*.cpp extension/*.hpp extension/*.h)
add_cpplint_target(ie_cpu_extension_cpplint FOR_SOURCES ${EXTENSION_SOURCES})
