project(PotreeConverter)
set(boost_min_ver 1.50.0)
set(boost_libs system filesystem)
set(Boost_USE_STATIC_LIBS ON)
find_package(Boost ${boost_min_ver})

if(Boost_FOUND)
    message( STATUS "Boost found!" )
    find_package(Boost ${boost_min_ver} COMPONENTS ${boost_libs})
endif()

set(RAPIDJSON_INCLUDE_DIR lib/rapidjson/include)
set(ARGUMENTS_INCLUDE_DIR lib/arguments)

file(GLOB_RECURSE files src/*.cpp include/*.h include/*.hpp )
file(GLOB_RECURSE lib_rapidjson_files ${RAPIDJSON_INCLUDE_DIR}/* )
file(GLOB_RECURSE lib_argument_files ${ARGUMENTS_INCLUDE_DIR}/* )

include_directories(include ${LASZIP_INCLUDE_DIRS})
include_directories(include ${RAPIDJSON_INCLUDE_DIR})
include_directories(include ${ARGUMENTS_INCLUDE_DIR})
include_directories(${Boost_INCLUDE_DIRS}) 

SOURCE_GROUP("Header Files\\io" FILES 
	include/BINPointReader.hpp 
	include/BINPointWriter.hpp
	include/LASPointReader.h
	include/LASPointWriter.hpp
	include/PlyPointReader.h
	include/PointReader.h
	include/PointWriter.hpp
	include/PTXPointReader.h
	include/XYZPointReader.hpp
)

SOURCE_GROUP("Source Files\\io" FILES 
	src/BINPointReader.cpp
	src/LASPointReader.cpp
	src/LASPointWriter.cpp
	src/PTXPointReader.cpp
)

SOURCE_GROUP("lib\\rapidjson" FILES ${lib_rapidjson_files})
SOURCE_GROUP("lib\\arguments" FILES ${lib_argument_files})

add_executable(PotreeConverter ${files} ${lib_rapidjson_files} ${lib_argument_files})

target_link_libraries(PotreeConverter ${Boost_LIBRARIES})
target_link_libraries(PotreeConverter ${LASZIP_LIBRARY})

if(UNIX)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++14 -pthread -lstdc++ -lm")
#    target_link_libraries(PotreeConverter -lstdc++fs)
endif()

install(TARGETS PotreeConverter RUNTIME DESTINATION bin/ )
