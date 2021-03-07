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

add_library(midifile ${MIDIFILE_SRCS})
target_include_directories(midifile PUBLIC ${MIDIFILE_INCS_DIR})

# ffmpeg
set(FFMPEG_INCS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg/ffmpeg-dev/include")
set(FFMPEG_LIBS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg/ffmpeg-dev/lib")
set(FFMPEG_LIBS avcodec avdevice avfilter avformat avutil postproc swresample swscale)
link_directories(${FFMPEG_LIBS_DIR})

# syn2midi
add_executable(syn2midi midi.cpp report.cpp getopt.c main.cpp)
target_include_directories(syn2midi PUBLIC ${CMAKE_CURRENT_SOURCE_DIR} ${FFMPEG_INCS_DIR} ${MIDIFILE_INCS_DIR})
target_link_libraries(syn2midi PUBLIC midifile ${FFMPEG_LIBS})
