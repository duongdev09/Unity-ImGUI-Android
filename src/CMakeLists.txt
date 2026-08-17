cmake_minimum_required(VERSION 3.18.1)
project("void")

add_link_options(-Wl,-z,max-page-size=16384)

add_library(
    void
    SHARED
    main.cpp
    utils.cpp
    hooks.cpp
    menu.cpp
    imgui/imgui.cpp
    imgui/imgui_demo.cpp
    imgui/imgui_draw.cpp
    imgui/imgui_tables.cpp
    imgui/imgui_widgets.cpp
    imgui/backends/imgui_impl_android.cpp
    imgui/backends/imgui_impl_opengl3.cpp
)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/libs/dobby/include)

find_library(log-lib log)

set(DOBBY_LIB
    ${CMAKE_CURRENT_SOURCE_DIR}/libs/dobby/${CMAKE_ANDROID_ARCH_ABI}/libdobby.a
)

target_compile_options(void PRIVATE
    -fPIC
    -O2
    -std=c++17
    -fvisibility=hidden
    -ffunction-sections
    -fdata-sections
    -Wno-error=format-security
)

target_link_options(void PRIVATE
    -Wl,-z,max-page-size=16384
    -Wl,--gc-sections
    -Wl,--strip-all
)

target_include_directories(void PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/backends
    ${CMAKE_CURRENT_SOURCE_DIR}/libs/dobby/include
)

target_link_libraries(void
    ${log-lib}
    ${DOBBY_LIB}
    android
    EGL
    GLESv3
    GLESv2
)
