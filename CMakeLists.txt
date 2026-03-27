cmake_minimum_required(VERSION 4.3.0)

# Get project version
file(READ "${CMAKE_SOURCE_DIR}/version.txt" VER_RAW)
string(REGEX MATCH "VERSION_MAJOR ([0-9]+)" _ "${VER_RAW}")
set(VER_MAJOR "${CMAKE_MATCH_1}")

string(REGEX MATCH "VERSION_MINOR ([0-9]+)" _ "${VER_RAW}")
set(VER_MINOR "${CMAKE_MATCH_1}")

string(REGEX MATCH "VERSION_PATCH ([0-9]+)" _ "${VER_RAW}")
set(VER_PATCH "${CMAKE_MATCH_1}")

set(PROJECT_VERSION "${VER_MAJOR}.${VER_MINOR}.${VER_PATCH}")

project(GarminFit
    VERSION ${PROJECT_VERSION}
    DESCRIPTION "GarminFIT Api"
    HOMEPAGE_URL https://developer.garmin.com/fit/overview/
    LANGUAGES CXX
)

include(FetchContent)
FetchContent_Declare(
  GarminSdk
  URL https://developer.garmin.com/downloads/fit/sdk/FitSDKRelease_21.158.00.zip
  SOURCE_DIR "${GARMIN_DIR}"
)
FetchContent_MakeAvailable(GarminSdk)
FetchContent_GetProperties(GarminSdk SOURCE_DIR GarminSdk_SOURCE_DIR)
message("Garmin Dir: ${GarminSdk_SOURCE_DIR}")
file(GLOB GarminSources "${GarminSdk_SOURCE_DIR}/cpp/*.cpp")
file(GLOB HEADER_FILES "${GarminSdk_SOURCE_DIR}/cpp/*.hpp")

add_library(${PROJECT_NAME} 
    ${GarminSources}
)

target_sources(${PROJECT_NAME}
    PRIVATE
        ${GarminSources}

    PUBLIC
        FILE_SET HEADERS
        BASE_DIRS "${GarminSdk_SOURCE_DIR}/cpp"
        FILES ${HEADER_FILES}
)
include (GNUInstallDirs)

install(
    TARGETS ${PROJECT_NAME}
    EXPORT GarminFit
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    FILE_SET HEADERS DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}"
)

install(
    EXPORT GarminFit DESTINATION lib/cmake/${PROJECT_NAME}
    NAMESPACE Garmin::
)