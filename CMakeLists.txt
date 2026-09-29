cmake_minimum_required(VERSION 3.16)
project(port)


set(ZLIB_INCLUDE_DIR "")


if(INCLUDE_AUDIO)

    add_subdirectory(base/id3lib)

endif()


if(INCLUDE_OPENGL)

    add_subdirectory(graphics3d/glad)

endif()


if(NOT ${HAS_SYSTEM_UNAC})

	add_subdirectory(base/unac)

endif()



