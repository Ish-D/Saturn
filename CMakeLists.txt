cmake_minimum_required(VERSION 3.27)
project(Saturn CXX)

find_package(Vulkan REQUIRED)

# Disable extra glfw building
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)

include(FetchContent)
FetchContent_Declare(fmt GIT_REPOSITORY https://github.com/fmtlib/fmt.git)
FetchContent_Declare(glfw3 GIT_REPOSITORY https://github.com/glfw/glfw.git)
FetchContent_Declare(VMA GIT_REPOSITORY https://github.com/GPUOpen-LibrariesAndSDKs/VulkanMemoryAllocator.git)
FetchContent_Declare(imgui URL https://github.com/ocornut/imgui/archive/v1.90.zip)
FetchContent_Declare(glm GIT_REPOSITORY https://github.com/g-truc/glm.git)
FetchContent_MakeAvailable(glfw3 fmt VMA imgui glm)

set(imgui_SOURCES
        ${imgui_SOURCE_DIR}/imgui.cpp
        ${imgui_SOURCE_DIR}/imgui_draw.cpp
        ${imgui_SOURCE_DIR}/imgui_tables.cpp
        ${imgui_SOURCE_DIR}/imgui_widgets.cpp
        ${imgui_SOURCE_DIR}/imgui_demo.cpp
        ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.cpp
        ${imgui_SOURCE_DIR}/backends/imgui_impl_vulkan.cpp)

add_executable(${PROJECT_NAME} src/main.cpp) # ${imgui_SOURCES}

target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra)
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_23)
target_compile_definitions(${PROJECT_NAME} PRIVATE GLM_FORCE_DEPTH_ZERO_TO_ONE)
target_link_libraries(${PROJECT_NAME} PRIVATE
        fmt::fmt
        glfw
        Vulkan::Vulkan $<$<BOOL:UNIX>:${CMAKE_DL_LIBS}>
        GPUOpen::VulkanMemoryAllocator
        glm::glm)