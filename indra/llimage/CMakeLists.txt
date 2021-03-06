# -*- cmake -*-

project(llimage)

include(00-Common)
include(LLCommon)
include(LLImage)
include(LLMath)
include(LLVFS)
include(ZLIB)

include_directories(
    ${LLCOMMON_INCLUDE_DIRS}
    ${LLMATH_INCLUDE_DIRS}
    ${LLVFS_INCLUDE_DIRS}
    ${PNG_INCLUDE_DIRS}
    ${ZLIB_INCLUDE_DIRS}
    )

set(llimage_SOURCE_FILES
    llimagebmp.cpp
    llimage.cpp
    llimagedxt.cpp
    llimagej2c.cpp
    llimagejpeg.cpp
    llimagemetadatareader.cpp
    llimagepng.cpp
    llimagetga.cpp
    llimageworker.cpp
    llpngwrapper.cpp
    )

set(llimage_HEADER_FILES
    CMakeLists.txt
    llimage.h
    llimagebmp.h
    llimagedxt.h
    llimagej2c.h
    llimagejpeg.h
    llimagemetadatareader.h
    llimagepng.h
    llimagetga.h
    llimageworker.h
    llmapimagetype.h
    llpngwrapper.h
    )

set_source_files_properties(${llimage_HEADER_FILES}
                            PROPERTIES HEADER_FILE_ONLY TRUE)

list(APPEND llimage_SOURCE_FILES ${llimage_HEADER_FILES})

add_library (llimage ${llimage_SOURCE_FILES})
# Libraries on which this library depends, needed for Linux builds
# Sort by high-level to low-level
target_link_libraries(llimage
    llcommon
    llimagej2coj        # *HACK: In theory a noop for KDU builds?
    ${JPEG_LIBRARIES}
    ${PNG_LIBRARIES}
    ${ZLIB_LIBRARIES}
    )

if (LL_TESTS)
  include(LLAddBuildTest)
  # Add tests
  ADD_BUILD_TEST(llimageworker llimage)
endif (LL_TESTS)
