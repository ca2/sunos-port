cmake_minimum_required(VERSION 3.16)
project(port)


set(ZLIB_INCLUDE_DIR "")

if(USE_PORT_FREEIMAGE)

    if(NOT USE_SYSTEM_JPEG)
        add_subdirectory(imaging/port_jpeg)
    endif()
    foreach(freeimage_codec jxr openexr openjpeg png tiff4 rawlite webp)
        add_subdirectory(imaging/port_${freeimage_codec})
    endforeach()
    add_subdirectory(imaging/port_freeimage)

    # The codec sources use namespaced headers (port_png/png.h, etc.).
    foreach(freeimage_port freeimage jpeg jxr openexr openjpeg png tiff4 rawlite webp)
        if(TARGET port_${freeimage_port})
            target_include_directories(port_${freeimage_port} PUBLIC
                "${CMAKE_CURRENT_SOURCE_DIR}/imaging/port_${freeimage_port}/include")
            if(__SUNOS__)
                target_compile_definitions(port_${freeimage_port} PRIVATE _REENTRANT)
                if(freeimage_port STREQUAL "freeimage" OR freeimage_port STREQUAL "png")
                    target_link_libraries(port_${freeimage_port} m)
                else()
                    target_link_libraries(port_${freeimage_port} PRIVATE m)
                endif()
            endif()
        endif()
    endforeach()

endif()


if(INCLUDE_AUDIO)

    add_subdirectory(base/id3lib)

endif()


if(INCLUDE_OPENGL)

    add_subdirectory(graphics3d/glad)

endif()



if(${USE_PORT_SQLITE})

    add_subdirectory(base/sqlite)

endif()



if(NOT ${HAS_SYSTEM_UNAC})

	add_subdirectory(base/unac)

endif()



