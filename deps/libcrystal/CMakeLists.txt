project(libcrystal)

include(CarrierDefaults)
include(ExternalProject)
include(ExternalCMakeArgs)

ExternalProject_Add(
    libcrystal

    PREFIX ${CARRIER_DEPS_BUILD_PREFIX}
    URL "https://github.com/iwhisperio/libcrystal/archive/master.zip"
    URL_HASH SHA256=f06159d155f4c2041ddc202f2a0107842f2edadca175d045f15f75014b16dd27
    DOWNLOAD_NAME "libcrystal-master.zip"
    DOWNLOAD_DIR ${CARRIER_DEPS_TARBALL_DIR}
    DOWNLOAD_NO_PROGRESS 1

    CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CARRIER_INT_DIST_DIR}
        -DENABLE_STATIC=${ENABLE_STATIC}
        -DENABLE_SHARED=${ENABLE_SHARED}
        -DLIBSODIUM_STATIC=TRUE
        -DENABLE_CRYPTO=TRUE
        -DENABLE_TESTS=FALSE
        ${CMAKE_ARGS_INIT}
)

if(ENABLE_SHARED)
    if(WIN32)
        set(INSTALL_DESTINATION bin)
    else()
        set(INSTALL_DESTINATION lib)
    endif()

    string(CONCAT SHARED_LIBRARY_NAME
        "${CARRIER_INT_DIST_DIR}/${INSTALL_DESTINATION}/"
        "${CMAKE_SHARED_LIBRARY_PREFIX}"
        "crystal"
        "${CMAKE_SHARED_LIBRARY_SUFFIX}")

    file(RELATIVE_PATH SHARED_LIBRARY_NAME ${CMAKE_CURRENT_LIST_DIR}
        ${SHARED_LIBRARY_NAME})

    install(PROGRAMS "${SHARED_LIBRARY_NAME}"
        DESTINATION ${INSTALL_DESTINATION})
endif()