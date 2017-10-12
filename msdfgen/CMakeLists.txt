cmake_minimum_required( VERSION 3.6.0 )

project ( msdfgen )

# find freetype
find_path(FREETYPE_INCLUDE_DIR ft2build.h)
if(CMAKE_BUILD_TYPE STREQUAL Debug)
    set(FT_SUFFIX d)
endif()
find_library(FREETYPE_LIBRARY freetype${FT_SUFFIX})

# OpenMP
FIND_PACKAGE( OpenMP REQUIRED)
if(OPENMP_FOUND)
message("OPENMP FOUND")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

add_library( msdfgen STATIC "" )
target_compile_options( msdfgen PRIVATE -Wall )

target_sources ( msdfgen
    PRIVATE
    ext/import-font.cpp
    ext/import-svg.cpp
    ext/save-png.cpp
    lib/lodepng.cpp
    lib/tinyxml2.cpp
    msdfCore/Bitmap.cpp
    msdfCore/Contour.cpp
    msdfCore/edge-coloring.cpp
    msdfCore/EdgeHolder.cpp
    msdfCore/edge-segments.cpp
    msdfCore/equation-solver.cpp
    msdfCore/msdfgen.cpp
    msdfCore/render-sdf.cpp
    msdfCore/save-bmp.cpp
    msdfCore/Shape.cpp
    msdfCore/shape-description.cpp
    msdfCore/SignedDistance.cpp
    msdfCore/Vector2.cpp 
)

target_include_directories( msdfgen PRIVATE 
    ./ext
    ./include
    ./msdfCore
    ${FREETYPE_INCLUDE_DIR}
)

target_compile_definitions( msdfgen PRIVATE NOMINMAX )
target_compile_features( msdfgen PRIVATE cxx_std_11 )

target_link_libraries( msdfgen PUBLIC 
    ${FREETYPE_LIBRARY}
)
