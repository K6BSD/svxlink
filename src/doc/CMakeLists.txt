# Add an empty doc target. We'll add dependencies later
add_custom_target(doc)

# Add targets to generate API documentation with Doxygen if it's installed
if(NOT QT_ONLY)
find_package(Doxygen)
if(DOXYGEN_FOUND)
  configure_file(doxygen.echolib.in doxygen.echolib @ONLY)
  add_custom_target(echolib-doc
    ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/doxygen.echolib
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "Generating EchoLib API documentation using Doxygen"
    VERBATIM
    )
  add_dependencies(doc echolib-doc)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/echolib/html/
    DESTINATION ${DOC_INSTALL_DIR}/echolib
    OPTIONAL)

  configure_file(doxygen.async.in doxygen.async @ONLY)
  add_custom_target(async-doc
    ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/doxygen.async
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "Generating Async API documentation using Doxygen"
    VERBATIM
    )
  add_dependencies(doc async-doc)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/async/html/
    DESTINATION ${DOC_INSTALL_DIR}/async
    OPTIONAL)

  set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES
    "echolib;async"
    )
endif(DOXYGEN_FOUND)
endif(NOT QT_ONLY)

add_subdirectory(man)
