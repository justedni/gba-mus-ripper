cmake_minimum_required(VERSION 3.16)
project(gba_mus_ripper LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/out)

if(MSVC)
    add_compile_options(/W3 /O1)
    add_compile_definitions(_CRT_SECURE_NO_WARNINGS _CRT_NONSTDC_NO_DEPRECATE)
else()
    add_compile_options(
        -Wall
        -fdata-sections
        -ffunction-sections
        -Os
    )
    if(APPLE)
        add_link_options(-Wl,-dead_strip)
    else()
        add_link_options(-Wl,--gc-sections -s)
    endif()
endif()


add_executable(sappy_detector
    sappy_detector.c
)

add_executable(song_ripper
    song_ripper.cpp
    midi.cpp
)

add_executable(sound_font_ripper
    sound_font_ripper.cpp
    gba_samples.cpp
    gba_instr.cpp
    sf2.cpp
)

add_executable(gba_mus_ripper
    gba_mus_ripper.cpp
)

install(TARGETS
    sappy_detector
    song_ripper
    sound_font_ripper
    gba_mus_ripper
    RUNTIME DESTINATION bin
)

install(FILES
    ${CMAKE_SOURCE_DIR}/psg_data.raw
    ${CMAKE_SOURCE_DIR}/goldensun_synth.raw
    DESTINATION bin
)
