cmake_minimum_required(VERSION 2.8.9)

project (fontue)

add_subdirectory( ./msdfgen msdfgen )

if(WIN32)

	# find freetype
	find_path(FREETYPE_INCLUDE_DIRS ft2build.h)
	if(CMAKE_BUILD_TYPE STREQUAL Debug)
	    set(FT_SUFFIX d)
	endif()
	find_library(FREETYPE_LIBRARIES freetype${FT_SUFFIX})

else()

	find_package(Freetype REQUIRED)

endif()

include_directories(${FREETYPE_INCLUDE_DIRS})

SET(CMAKE_CXX_FLAGS "-O2 -std=c++11")
SET(CMAKE_EXE_LINKER_FLAGS "-v")

file(GLOB SOURCES "src/*.cpp")
add_executable(fontue ${SOURCES})

target_include_directories( fontue PRIVATE 
								   ${FREETYPE_INCLUDE_DIRS}
								   ./msdfgen )
target_link_libraries(fontue ${FREETYPE_LIBRARIES} msdfgen )
