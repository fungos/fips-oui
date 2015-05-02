#
# project: fips-oui
#

if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 2.8)
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")
    fips_setup()
    fips_project(fips-oui)
endif()

add_definitions(-DNANOVG_GLEW)
add_definitions(-DDEMO_MSAA)
add_definitions(-DGLEW_STATIC) # tell glew headers we are static linking with glew

if (FIPS_MACOS OR FIPS_LINUX OR FIPS_WINDOWS)
    fips_begin_app(blendish cmdline)
        include_directories(oui-blendish)
        fips_dir(oui-blendish)
        fips_files(example.cpp)
        fips_deps(nanovg GLEW glfw3)

        if (FIPS_WINDOWS)
            fips_libs(opengl32)
        endif()

        if (FIPS_LINUX)
            fips_libs(X11 Xrandr Xi Xinerama Xxf86vm Xcursor GL m)
        endif()

        if (FIPS_MACOS)
            fips_libs(m)
        endif()
    fips_end_app()
endif()

if (NOT FIPS_IMPORT)
    fips_finish()
endif()

