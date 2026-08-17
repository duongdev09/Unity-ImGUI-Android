cmake_minimum_required(VERSION 3.18.1)

project("void")

# ─── Android 15 / 16KB page size fix ───────────────────────────────────────
add_link_options(-Wl,-z,max-page-size=16384)

# ─── Source files (giữ nguyên của mày) ─────────────────────────────────────
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
    imgui/imgui_impl_android.cpp
    imgui/imgui_impl_opengl3.cpp
)

# ─── Dobby (giữ nguyên của mày) ────────────────────────────────────────────
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/libs/dobby/include)

set(DOBBY_LIB
    ${CMAKE_CURRENT_SOURCE_DIR}/libs/dobby/${CMAKE_ANDROID_ARCH_ABI}/libdobby.a
)

find_library(log-lib log)

# ─── Compile flags (cái này mày đang thiếu — thêm vào) ─────────────────────
target_compile_options(void PRIVATE
    -fPIC
    -O2
    -std=c++17
    -fvisibility=hidden
    -ffunction-sections
    -fdata-sections
    -Wno-error=format-security
)

# ─── Linker flags ───────────────────────────────────────────────────────────
target_link_options(void PRIVATE
    -Wl,-z,max-page-size=16384
    -Wl,--gc-sections
    -Wl,--strip-all
)

# ─── Include dirs ───────────────────────────────────────────────────────────
target_include_directories(void PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui
    ${CMAKE_CURRENT_SOURCE_DIR}/libs/dobby/include
)

# ─── Link libs (giữ nguyên + thêm GLESv2 fallback) ─────────────────────────
target_link_libraries(void
    ${log-lib}
    ${DOBBY_LIB}
    android
    EGL
    GLESv3
    GLESv2
)
