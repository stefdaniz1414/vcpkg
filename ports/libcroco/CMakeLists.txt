cmake_minimum_required(VERSION 3.11)
project(libcroco C)

find_package(unofficial-glib CONFIG REQUIRED)
find_package(LibXml2 REQUIRED)
if(NOT WIN32)
    find_package(Threads REQUIRED)
    find_package(unofficial-iconv REQUIRED)
endif()
find_path(GLIB_INCLUDE_DIR glib.h)

file(GLOB SOURCES
	src/cr-utils.c
	src/cr-utils.h
	src/cr-input.c
	src/cr-input.h
	src/cr-enc-handler.c
	src/cr-enc-handler.h
	src/cr-num.c
	src/cr-num.h
	src/cr-rgb.c
	src/cr-rgb.h
	src/cr-token.c
	src/cr-token.h
	src/cr-tknzr.c
	src/cr-tknzr.h
	src/cr-term.c
	src/cr-term.h
	src/cr-attr-sel.c
	src/cr-attr-sel.h
	src/cr-pseudo.c
	src/cr-pseudo.h
	src/cr-additional-sel.c
	src/cr-additional-sel.h
	src/cr-simple-sel.c
	src/cr-simple-sel.h
	src/cr-selector.c
	src/cr-selector.h
	src/cr-doc-handler.c
	src/cr-doc-handler.h
	src/cr-parser.c
	src/cr-parser.h
	src/cr-declaration.c
	src/cr-declaration.h
	src/cr-statement.c
	src/cr-statement.h
	src/cr-stylesheet.c
	src/cr-stylesheet.h
	src/cr-cascade.c
	src/cr-cascade.h
	src/cr-om-parser.c
	src/cr-om-parser.h
	src/cr-style.c
	src/cr-style.h
	src/cr-sel-eng.c
	src/cr-sel-eng.h
	src/cr-fonts.c
	src/cr-fonts.h
	src/cr-prop-list.c
	src/cr-prop-list.h
	src/cr-parsing-location.c
	src/cr-parsing-location.h
	src/cr-string.c
	src/cr-string.h
	src/libcroco.def
)

set(CMAKE_DEBUG_POSTFIX "d")

add_library(libcroco ${SOURCES})

target_include_directories(libcroco PRIVATE ${GLIB_INCLUDE_DIR} ${LIBXML2_INCLUDE_DIR})
target_link_libraries(libcroco PRIVATE 
    unofficial::glib::gio
    unofficial::glib::glib
    unofficial::glib::gmodule
    unofficial::glib::gobject
    ${LIBXML2_LIBRARIES}
)

install(TARGETS libcroco
    EXPORT libcroco-targets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)

install(FILES
	src/libcroco.h
	src/cr-additional-sel.h
	src/cr-attr-sel.h
	src/cr-cascade.h
	src/cr-declaration.h
	src/cr-doc-handler.h
	src/cr-enc-handler.h
	src/cr-input.h
	src/cr-num.h
	src/cr-om-parser.h
	src/cr-parser.h
	src/cr-pseudo.h
	src/cr-rgb.h
	src/cr-selector.h
	src/cr-simple-sel.h
	src/cr-statement.h
	src/cr-stylesheet.h
	src/cr-term.h
	src/cr-tknzr.h
	src/cr-token.h
	src/cr-utils.h
	src/cr-fonts.h
	src/cr-sel-eng.h
	src/cr-style.h
	src/cr-prop-list.h
	src/cr-parsing-location.h
	src/cr-string.h
	src/libcroco-config.h
    DESTINATION include/libcroco
)

install(
    EXPORT libcroco-targets
    NAMESPACE unofficial::libcroco::
    FILE unofficial-libcroco-targets.cmake
    DESTINATION share/unofficial-libcroco
)

file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/unofficial-libcroco-config.cmake "
include(CMakeFindDependencyMacro)
find_dependency(unofficial-glib CONFIG)
find_dependency(LibXml2)
if(NOT WIN32)
    find_dependency(Threads)
    find_dependency(unofficial-iconv)
endif()
include(\${CMAKE_CURRENT_LIST_DIR}/unofficial-libcroco-targets.cmake)
")
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/unofficial-libcroco-config.cmake DESTINATION share/unofficial-libcroco)
