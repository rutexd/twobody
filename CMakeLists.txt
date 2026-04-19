cmake_minimum_required(VERSION 3.24)
project(twobody LANGUAGES C)


option(TWOBODY_USE_NATIVE "Use -march=native" OFF)
option(TWOBODY_ENABLE_AVX "Enable AVX2" ON)


set(SOURCES
    src/twobody/anomaly.c
    src/twobody/conic.c
    src/twobody/eccentric_anomaly.c
    src/twobody/fg.c
    src/twobody/orbit.c
    src/twobody/orientation.c
    src/twobody/stumpff.c
    src/twobody/true_anomaly.c
    src/twobody/twobody.c
    src/twobody/universal.c
)

set(HEADERS
    include/twobody/anomaly.h
    include/twobody/conic.h
    include/twobody/eccentric_anomaly.h
    include/twobody/fg.h
    include/twobody/math_utils.h
    include/twobody/orbit.h
    include/twobody/orientation.h
    include/twobody/simd4d.h
    include/twobody/stumpff.h
    include/twobody/true_anomaly.h
    include/twobody/twobody.h
    include/twobody/universal.h
)

add_library(twobody STATIC ${SOURCES} ${HEADERS})

target_include_directories(twobody PUBLIC include)

if(TWOBODY_USE_NATIVE)
    target_compile_options(twobody PUBLIC -march=native)
elseif(TWOBODY_ENABLE_AVX)
    target_compile_options(twobody PUBLIC -mavx2)
else()
    target_compile_definitions(twobody PUBLIC TWOBODY_NO_SIMD)
endif()

if(NOT WIN32)
    target_link_libraries(twobody PRIVATE m)
endif()
