cmake_minimum_required(VERSION 3.16)
project(glfw LANGUAGES C)

add_library(glfw include/GLFW/glfw3.h 
                    include/GLFW/glfw3native.h 
                    src/internal.h 
                    src/platform.h 
                    src/mappings.h 
                    src/context.c 
                    src/init.c 
                    src/input.c 
                    src/monitor.c 
                    src/platform.c 
                    src/vulkan.c 
                    src/window.c 
                    src/egl_context.c 
                    src/osmesa_context.c 
                    src/null_platform.h 
                    src/null_joystick.h 
                    src/null_init.c 
                    src/null_monitor.c 
                    src/null_window.c 
                    src/null_joystick.c)
                    
if (WIN32)
    target_sources(glfw PRIVATE src/win32_init.c 
                            src/win32_module.c 
                            src/win32_joystick.c 
                            src/win32_monitor.c 
                            src/win32_time.h 
                            src/win32_time.c 
                            src/win32_thread.c 
                            src/win32_thread.h 
                            src/win32_window.c 
                            src/wgl_context.c 
                            src/egl_context.c 
                            src/osmesa_context.c)
    target_compile_definitions(glfw PRIVATE _GLFW_WIN32)                       
else()
    target_sources(glfw PRIVATE src/x11_init.c 
                            src/posix_module.c 
                            src/linux_joystick.c 
                            src/x11_monitor.c 
                            src/posix_time.h 
                            src/posix_time.c 
                            src/posix_thread.c 
                            src/posix_thread.h 
                            src/x11_window.c 
                            src/glx_context.c 
                            src/egl_context.c 
                            src/osmesa_context.c
                            src/posix_poll.h 
                            src/posix_poll.c
                            src/xkb_unicode.h
                            src/xkb_unicode.c
                            src/x11_platform.h)

    target_compile_definitions(glfw PRIVATE _DEFAULT_SOURCE)    
    target_compile_definitions(glfw PRIVATE _GLFW_X11)

    find_package(X11 REQUIRED)
    target_include_directories(glfw PRIVATE "${X11_X11_INCLUDE_PATH}")

    # Check for XRandR (modern resolution switching and gamma control)
    if (NOT X11_Xrandr_INCLUDE_PATH)
        message(FATAL_ERROR "RandR headers not found; install libxrandr development package")
    endif()
    target_include_directories(glfw PRIVATE "${X11_Xrandr_INCLUDE_PATH}")

    # Check for Xinerama (legacy multi-monitor support)
    if (NOT X11_Xinerama_INCLUDE_PATH)
        message(FATAL_ERROR "Xinerama headers not found; install libxinerama development package")
    endif()
    target_include_directories(glfw PRIVATE "${X11_Xinerama_INCLUDE_PATH}")

    # Check for Xkb (X keyboard extension)
    if (NOT X11_Xkb_INCLUDE_PATH)
        message(FATAL_ERROR "XKB headers not found; install X11 development package")
    endif()
    target_include_directories(glfw PRIVATE "${X11_Xkb_INCLUDE_PATH}")

    # Check for Xcursor (cursor creation from RGBA images)
    if (NOT X11_Xcursor_INCLUDE_PATH)
        message(FATAL_ERROR "Xcursor headers not found; install libxcursor development package")
    endif()
    target_include_directories(glfw PRIVATE "${X11_Xcursor_INCLUDE_PATH}")

    # Check for XInput (modern HID input)
    if (NOT X11_Xi_INCLUDE_PATH)
        message(FATAL_ERROR "XInput headers not found; install libxi development package")
    endif()
    target_include_directories(glfw PRIVATE "${X11_Xi_INCLUDE_PATH}")

    # Check for X Shape (custom window input shape)
    if (NOT X11_Xshape_INCLUDE_PATH)
        message(FATAL_ERROR "X Shape headers not found; install libxext development package")
    endif()
    target_include_directories(glfw PRIVATE "${X11_Xshape_INCLUDE_PATH}")

    target_compile_options(glfw PRIVATE "-fvisibility=hidden")

    

endif (WIN32)

target_compile_definitions(glfw PRIVATE _CRT_SECURE_NO_WARNINGS)

set(imgui_macro "__declspec(dllexport)")
add_compile_definitions(IMGUI_API=${imgui_macro}) 


install(TARGETS glfw RUNTIME DESTINATION /bin/${CMAKE_PROJECT_NAME})