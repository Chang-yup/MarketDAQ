cmake_minimum_required(VERSION 3.10)
project(MarketDAQ)
set(CMAKE_CXX_STANDARD 17)

# build option
option(RELEASE "Build RELEASE" OFF)
option(TEST_IMGUI "Build TEST_IMGUI" OFF)
option(TEST_IMGUI "Build TEST_CURL" OFF)
option(TEST_FSTREAM "Build TEST_FSTREAM" OFF)
option(TEST_JSON "Build TEST_JSON" OFF)

# list for dependenciy management
set(BUILD_LIBRARIES)

# dependency management by build option
if(RELEASE)
    list(APPEND BUILD_LIBRARIES imgui curl)
endif()
if(TEST_IMGUI)
    list(APPEND BUILD_LIBRARIES imgui)
endif()
if(TEST_CURL)
    list(APPEND BUILD_LIBRARIES curl)
endif()
if(TEST_FSTREAM)
    list(APPEND BUILD_LIBRARIES json)
endif()
if(TEST_JSON)
    list(APPEND BUILD_LIBRARIES json)
endif()

# add library directory to project
# set INCLUDE_DIRECTORIES, LINK_DIRECTORIES, LINK_LIBRARIES in 'lib/CMakeLists.txt'
add_subdirectory(lib)

# target definition by option
if(RELEASE)
    message(STATUS "EXECUTABLE TARGET: RELEASE")
    add_executable(MarketDAQ src/main.cpp src/gui.cpp)
endif()
if(TEST_IMGUI)
    message(STATUS "EXECUTABLE TARGET: TEST_IMGUI")
    add_executable(MarketDAQ test/TEST_IMGUI.cpp)
endif()
if(TEST_CURL)
    message(STATUS "EXECUTABLE TARGET: TEST_CURL")
    add_executable(MarketDAQ test/TEST_CURL.cpp)
endif()
if(TEST_FSTREAM)
    message(STATUS "EXECUTABLE TARGET: TEST_FSTREAM")
    add_executable(MarketDAQ test/TEST_FSTREAM.cpp)
endif()
if(TEST_JSON)
    message(STATUS "EXECUTABLE TARGET: TEST_JSON")
    add_executable(MarketDAQ test/TEST_JSON.cpp src/type.cpp)
endif()

target_include_directories(MarketDAQ PUBLIC src/ include/ ${INCLUDE_DIRECTORIES})
target_link_directories(MarketDAQ PRIVATE ${LINK_DIRECTORIES})
target_link_libraries(MarketDAQ PRIVATE ${LINK_LIBRARIES})