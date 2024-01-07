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
                            src/osmesa_context.c)
    target_compile_definitions(glfw PRIVATE _DEFAULT_SOURCE)    
    target_compile_definitions(glfw PRIVATE _GLFW_X11)
endif (WIN32)

target_compile_definitions(glfw PRIVATE _CRT_SECURE_NO_WARNINGS)

set(imgui_macro "__declspec(dllexport)")
add_compile_definitions(IMGUI_API=${imgui_macro}) 


install(TARGETS glfw RUNTIME DESTINATION /bin/${CMAKE_PROJECT_NAME})