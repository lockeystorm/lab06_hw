cmake_minimum_required(VERSION 3.30)
project(solver)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(VERSION_MAJOR 0)
set(VERSION_MINOR 1)
set(VERSION_PATCH 0)
set(VERSION_TWEAK 0)
set(VERSION ${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}.${VERSION_TWEAK})
set(STRING_VERSION "v${VERSION}")

add_subdirectory(formatter_lib)
add_subdirectory(formatter_ex_lib)
add_subdirectory(solver_lib)
add_subdirectory(solver_application)

install(TARGETS formatter formatter_ex solver_lib
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    INCLUDES DESTINATION include
)

install(DIRECTORY 
    formatter_lib/
    formatter_ex_lib/
    solver_lib/
    DESTINATION include
    FILES_MATCHING PATTERN "*.h"
)

include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_NAME "solver")
set(CPACK_PACKAGE_VERSION ${VERSION})
set(CPACK_PACKAGE_VENDOR "company")
set(CPACK_PACKAGE_CONTACT "email")
set(CPACK_PACKAGE_DESCRIPTION "Quadratic equation solver")
set(CPACK_RESOURCE_FILE_README "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
if (WIN32)
    set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE.rtf")
else()
    set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")
endif()

set(CPACK_GENERATOR "TGZ;ZIP")
if(WIN32)
    set(CPACK_GENERATOR "WIX")
elseif(APPLE)
    set(CPACK_GENERATOR "${CPACK_GENERATOR};DragNDrop")
elseif(UNIX)
    set(CPACK_GENERATOR "${CPACK_GENERATOR};DEB;RPM")
endif()

# Linux (DEB/RPM)
set(CPACK_DEBIAN_PACKAGE_SECTION "devel")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "owner")
set(CPACK_DEBIAN_PACKAGE_DEPENDS "libc6 (>= 2.14), libstdc++6 (>= 4.8.1)")
set(CPACK_DEBIAN_FILE_NAME DEB-DEFAULT)
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)

set(CPACK_RPM_PACKAGE_RELEASE 1)
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_REQUIRES "libstdc++ >= 4.8.1")

# Windows (MSI)
set(CPACK_WIX_PRODUCT_GUID "0eec58d0-3ea2-4778-bb6a-4eabfe6338a0")
set(CPACK_WIX_UPGRADE_GUID "18ad7e65-2d71-4072-ac5f-25ddde61e8a9")
set(CPACK_WIX_ARCHITECTURE "x64")

# macOS (DMG)
set(CPACK_DMG_VOLUME_NAME "Solver")
set(CPACK_DMG_FORMAT "UDBZ")

include(CPack)


