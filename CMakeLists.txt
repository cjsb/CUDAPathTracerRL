cmake_minimum_required(VERSION 3.8 FATAL_ERROR)
project(PathTracer_CUDA LANGUAGES CXX CUDA)

if ($ENV{CLION_IDE})
    include_directories("C:\\Program Files\\NVIDIA GPU Computing Toolkit\\CUDA\\v8.0\\include")
endif ()

set(WIN32 1)

set(CMAKE_CXX_STANDARD 11)

add_library(cuda_base
        cudaMain.cu cudaMain.h
        camera.cpp camera.h
        color.cpp color.h
        gl_utils.cpp gl_utils.h
        lightsource.h
        material.cpp material.h
        object.h
        pointlightsource.h
        quadric.cpp quadric.h
        ray.cpp ray.h
        renderengine.cpp renderengine.h
        renderengine_gpu.cu renderengine_gpu.h
        shader_utils.cpp shader_utils.h
        sphere.cpp sphere.h
        structs.h
        triangle.cpp triangle.h
        vector3D.cpp vector3D.h
        world.cpp world.h
        cutil_math.h
        dummy.cpp
        cudaHeaders.h pathtracer_params.h
        camera_gpu.h world_gpu.h
        sphere_gpu.h ray_gpu.h
        cuda_utils.h scene_helper.h
        material_functions.h
        rl_helper_functions.h
        )

target_compile_features(cuda_base PUBLIC cxx_std_11)
set_target_properties( cuda_base
        PROPERTIES CUDA_SEPARABLE_COMPILATION ON
        )



find_package(OpenGL REQUIRED)
add_subdirectory (external)
include_directories(
        external/glfw-3.1.2/include/
        external/glm-0.9.7.1/
        .
)

set(OPENGL_RELATED_LIBS
        ${OPENGL_LIBRARY}
        glfw
        glew32s
        )
add_definitions(
        -DTW_STATIC
        -DTW_NO_LIB_PRAGMA
        -DTW_NO_DIRECT3D
        -DGLEW_STATIC
        -D_CRT_SECURE_NO_WARNINGS
)



set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -gencode arch=compute_50,code=sm_50")

add_executable(PathTracer_CUDA
        main.cpp
        )

set_target_properties(
        PathTracer_CUDA
        PROPERTIES
            CUDA_SEPARABLE_COMPILATION ON
    )
target_link_libraries(PathTracer_CUDA cuda_base ${OPENGL_RELATED_LIBS})
