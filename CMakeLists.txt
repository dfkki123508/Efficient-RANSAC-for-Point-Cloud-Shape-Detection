# Taken from https://github.com/ihmcrobotics/ihmc-open-robotics-software/blob/5f5345ea78f681c1ca815bb1539041b5d0ab54d0/ihmc-sensor-processing/csrc/ransac_schnabel/CMakeLists.txt
cmake_minimum_required(VERSION 2.8)
project(pc_ransac)

set(CMAKE_CXX_FLAGS "-fpermissive")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)
#include_directories("${CMAKE_CURRENT_SOURCE_DIR}/GfxTL/")
#include_directories("${CMAKE_CURRENT_SOURCE_DIR}/MiscLib/")
FILE(GLOB SRC "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")
FILE(GLOB MISC_SRC "${CMAKE_CURRENT_SOURCE_DIR}/src/MiscLib/*.cpp")
FILE(GLOB_RECURSE HEADERS "./src/**.h*")
message(STATUS  "Header files: ${HEADERS}")

if(WIN32)
    macro(get_WIN32_WINNT version)
        if(CMAKE_SYSTEM_VERSION)
            set(ver ${CMAKE_SYSTEM_VERSION})
            string(REGEX MATCH "^([0-9]+).([0-9])" ver ${ver})
            string(REGEX MATCH "^([0-9]+)" verMajor ${ver})
            # Check for Windows 10, b/c we'll need to convert to hex 'A'.
            if("${verMajor}" MATCHES "10")
                set(verMajor "A")
                string(REGEX REPLACE "^([0-9]+)" ${verMajor} ver ${ver})
            endif()
            # Remove all remaining '.' characters.
            string(REPLACE "." "" ver ${ver})
            # Prepend each digit with a zero.
            string(REGEX REPLACE "([0-9A-Z])" "0\\1" ver ${ver})
            set(${version} "0x${ver}")
        endif()
    endmacro()

    get_WIN32_WINNT(ver)
    add_definitions(-D_WIN32_WINNT=${ver})
endif()

add_compile_definitions(-D _SILENCE_STDEXT_HASH_DEPRECATION_WARNINGS)

add_library(libransac STATIC ${SRC} ${MISC_SRC})
add_executable(main "${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp")
target_link_libraries(main libransac)

install(DIRECTORY src/ DESTINATION include FILES_MATCHING PATTERN "*.h[pp]?")
install(TARGETS libransac DESTINATION lib)