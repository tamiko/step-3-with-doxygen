SET(TARGET "step-3")

SET(TARGET_SRC ${TARGET}.cc)

CMAKE_MINIMUM_REQUIRED(VERSION 3.1.0)

FIND_PACKAGE(deal.II 9.3.0
  HINTS ${deal.II_DIR} ${DEAL_II_DIR} ../ ../../ $ENV{DEAL_II_DIR}
  )
IF(NOT ${deal.II_FOUND})
  MESSAGE(FATAL_ERROR "\n"
    "*** Could not locate a (sufficiently recent) version of deal.II. ***\n\n"
    "You may want to either pass a flag -DDEAL_II_DIR=/path/to/deal.II to cmake\n"
    "or set an environment variable \"DEAL_II_DIR\" that contains this path."
    )
ENDIF()

DEAL_II_INITIALIZE_CACHED_VARIABLES()
PROJECT(${TARGET})
DEAL_II_INVOKE_AUTOPILOT()

#
# Doxygen configuration:
#

add_custom_command(
  OUTPUT ${CMAKE_BINARY_DIR}/doxygen.log
  COMMAND doxygen ${CMAKE_BINARY_DIR}/doxygen.conf
    > ${CMAKE_BINARY_DIR}/doxygen.log
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  DEPENDS
    ${TARGET_SRC}
    ${CMAKE_BINARY_DIR}/doxygen.conf
  COMMENT "Generating documentation via doxygen."
  VERBATIM
  )

add_custom_target(doxygen ALL
  DEPENDS ${CMAKE_BINARY_DIR}/doxygen.log
  )
