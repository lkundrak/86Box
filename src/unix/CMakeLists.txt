set(PLAT_SOURCES unix_thread.c)
if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    find_package(ALSA)
    if (ALSA_FOUND)
        set(PLAT_SOURCES ${PLAT_SOURCES} linux_midi_alsa.c)
    else()
        set(PLAT_SOURCES ${PLAT_SOURCES} unix_midi.c)
    endif()
else()
    set(PLAT_SOURCES ${PLAT_SOURCES} unix_midi.c)
endif()
add_library(plat STATIC ${PLAT_SOURCES})
add_library(ui STATIC unix.c unix_sdl.c unix_cdrom.c)
target_compile_definitions(ui PUBLIC _FILE_OFFSET_BITS=64)
target_link_libraries(ui dl)
target_link_libraries(plat pthread)
if (ALSA_FOUND)
    target_link_libraries(plat ALSA::ALSA)
endif()