cmake_minimum_required(VERSION 3.0)

project(syn2midi)

# midifile
set(MIDIFILE_INCS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/midifile-master/include")
set(MIDIFILE_SRCS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/midifile-master/src-library")
set(MIDIFILE_SRCS
    ${MIDIFILE_SRCS_DIR}/Binasc.cpp
    ${MIDIFILE_SRCS_DIR}/MidiEvent.cpp
    ${MIDIFILE_SRCS_DIR}/MidiEventList.cpp
    ${MIDIFILE_SRCS_DIR}/MidiFile.cpp
    ${MIDIFILE_SRCS_DIR}/MidiMessage.cpp
    ${MIDIFILE_SRCS_DIR}/Options.cpp
)

add_library(midifile STATIC ${MIDIFILE_SRCS})
target_include_directories(midifile PUBLIC ${MIDIFILE_INCS_DIR})

# ffmpeg
set(FFMPEG_INCS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg/ffmpeg-dev/include")
set(FFMPEG_LIBS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg/ffmpeg-dev/lib")
set(FFMPEG_LIBS avcodec avdevice avfilter avformat avutil postproc swresample swscale)

foreach(FFMPEG_LIB ${FFMPEG_LIBS})
    add_library(${FFMPEG_LIB} STATIC IMPORTED)
    set_target_properties(${FFMPEG_LIB} 
        PROPERTIES 
        IMPORTED_LOCATION "${FFMPEG_LIBS_DIR}/${FFMPEG_LIB}.lib"
    )
endforeach()

# syn2midi
set(SYN2MIDI_SRCS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/syn2midi")
add_executable(syn2midi
    ${SYN2MIDI_SRCS_DIR}/midi.cpp
    ${SYN2MIDI_SRCS_DIR}/report.cpp
    ${SYN2MIDI_SRCS_DIR}/getopt.c
    ${SYN2MIDI_SRCS_DIR}/main.cpp
)
target_include_directories(syn2midi PUBLIC ${SYN2MIDI_SRCS_DIR} ${FFMPEG_INCS_DIR} ${MIDIFILE_INCS_DIR})
target_link_libraries(syn2midi PRIVATE midifile ${FFMPEG_LIBS})
