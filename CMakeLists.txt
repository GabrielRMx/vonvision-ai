cmake_minimum_required(VERSION 3.5)

project(vonvision-ai LANGUAGES CXX)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories(${CMAKE_SOURCE_DIR})

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 --permissive -D_FILE_OFFSET_BITS=64 -DREENTRANT")

include_directories(
    /home/gabriel/BUILDROOT/buildroot/output/host/aarch64-buildroot-linux-gnu/include/c++/11.3.0/aarch64-buildroot-linux-gnu
    /home/gabriel/BUILDROOT/buildroot/output/host/aarch64-buildroot-linux-gnu/include/c++/11.3.0
    /home/gabriel/BUILDROOT/buildroot/output/target/usr/include/SDL2
    /home/gabriel/BUILDROOT/buildroot/output/staging/usr/include/drm
)

set(SDL_FONTCACHE_LIBRARIES "/home/gabriel/BUILDROOT/buildroot/output/target/usr/lib/libSDL_FontCache.a")

# Comentado mientras se use otro metodo para llamar a las libs de FFMPEG
# set(FFMPEG_LIBS
#     avutil
#     avcodec
#     avformat
#     avdevice
#     avfilter
#     swscale
#     swresample
#     postproc
# )

set(OTHER_LIBS
    SDL2_ttf
    rockchip_mpp
    rga
    vorbis
    vorbisenc
    tiff
    opus
    ogg
    mp3lame
    lzma
    rtmp
    ssl
    crypto
    bz2
    xml2
    X11
    xcb
    Xv
    Xext
    v4l2
    asound
    pulse
    GLESv2
    freetype
    xcb
    xcb-shm
    xcb-xfixes
    xcb-render
    xcb-shape
    rknnrt
)

find_package(PkgConfig REQUIRED)
# pkg_check_modules(FFMPEG REQUIRED libavcodec libavformat libavutil libswscale)
pkg_check_modules(FFMPEG REQUIRED libavdevice libavfilter libavformat #[[libavresample swresample]] libswscale libavcodec libavutil)

include_directories(${FFMPEG_INCLUDE_DIRS})
link_directories(${FFMPEG_LIBRARY_DIRS})

# # Usar pkg-config para SDL3
# find_package(PkgConfig REQUIRED)
# pkg_check_modules(SDL3 REQUIRED sdl3)

# include_directories(${SDL3_INCLUDE_DIRS})
# link_directories(${SDL3_LIBRARY_DIRS})

# Usar pkg-config para SDL2
find_package(PkgConfig REQUIRED)
pkg_check_modules(SDL2 REQUIRED sdl2)

include_directories(${SDL2_INCLUDE_DIRS})
link_directories(${SDL2_LIBRARY_DIRS})

# Usar pkg-config para OpenCV
find_package( OpenCV REQUIRED )
include_directories( ${OpenCV_INCLUDE_DIRS} )

set(SOURCES
    main.cpp
    mainprogram.cpp
    sdlwidget.cpp
    postprocess.cpp
    ffrknn.cpp
    postprocess_ocr.cc
    ppocr_rec.cc
)

set(HEADERS
    mainprogram.h
    sdlwidget.h
    postprocess.h
    ffrknn.h
    ppocr_rec.h
)

set(FORMS
    mainprogram.ui
)

add_executable(vonvision-ai
    ${SOURCES}
    ${HEADERS}
    ${FORMS}


)

# Linkear las librerías necesarias
target_link_libraries(vonvision-ai
    Qt${QT_VERSION_MAJOR}::Widgets
    ${FFMPEG_LIBRARIES}
    ${OTHER_LIBS}
    # ${SDL3_LIBRARIES}
    ${SDL2_LIBRARIES}
    ${SDL_FONTCACHE_LIBRARIES}
    ${OpenCV_LIBS}
    z
    m
    pthread
)

