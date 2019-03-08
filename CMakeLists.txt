cmake_minimum_required(VERSION 2.8)

project(adaptagrams)

install(FILES cmake/adaptagramsConfig.cmake DESTINATION share/adaptagrams/cmake)
install(EXPORT adaptagrams NAMESPACE adaptagrams:: DESTINATION share/adaptagrams/cmake)

include_directories(cola)

macro(library_from_makefile_am LIBRARY_NAME)
  file(READ Makefile.am MAKEFILE_AM)
  string(REGEX MATCHALL "[a-z]*\\.cpp" SOURCES "${MAKEFILE_AM}")

  string(FIND "${MAKEFILE_AM}" "include_HEADERS" START)
  string(SUBSTRING "${MAKEFILE_AM}" "${START}" "-1" MAKEFILE_AM)
  string(REGEX MATCHALL "[a-z]*\\.h" HEADERS "${MAKEFILE_AM}")
  message(START "${START}")
  message(HEADERS "${HEADERS}")

  add_library("${LIBRARY_NAME}" SHARED ${SOURCES})

  get_filename_component(INSTALL_PATH "${CMAKE_CURRENT_SOURCE_DIR}" NAME)
  install(FILES ${HEADERS} DESTINATION include/${INSTALL_PATH})

  install(TARGETS "${LIBRARY_NAME}" EXPORT adaptagrams LIBRARY DESTINATION lib/)
endmacro(library_from_makefile_am)

add_subdirectory(cola/libavoid)
