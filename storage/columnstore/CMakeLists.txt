MYSQL_ADD_PLUGIN(columnstore STORAGE_ENGINE
    MODULE_ONLY MODULE_OUTPUT_NAME engine/dbcon/mysql/.libs/libcalmysql.so)

IF(NOT TARGET columnstore)
  RETURN()
ENDIF()

include(ExternalProject)
ExternalProject_Add(columnstore_engine
 SOURCE_DIR ${CMAKE_SOURCE_DIR}/storage/columnstore/engine
 BUILD_IN_SOURCE 1
 CONFIGURE_COMMAND build/bootstrap --prefix=${CMAKE_INSTALL_PREFIX} --include=${CMAKE_BINARY_DIR}/include --include=${CMAKE_BINARY_DIR}/sql --include=${CMAKE_BINARY_DIR}/pcre --include=${CMAKE_SOURCE_DIR}/sql 
 INSTALL_COMMAND build/install --prefix=${CMAKE_INSTALL_PREFIX}
)

ADD_DEPENDENCIES(columnstore columnstore_engine) 
