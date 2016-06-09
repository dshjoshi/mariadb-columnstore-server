MYSQL_ADD_PLUGIN(columnstore STORAGE_ENGINE
    MODULE_ONLY MODULE_OUTPUT_NAME engine/dbcon/mysql/.libs/libcalmysql.so)

IF(NOT TARGET columnstore)
  RETURN()
ENDIF()

include(ExternalProject)
ExternalProject_Add(columnstore_engine 
  SOURCE_DIR engine
  BINARY_DIR engine
  CONFIGURE_COMMAND build/bootstrap --prefix=${CMAKE_INSTALL_PREFIX}
  INSTALL_COMMAND make install DESTDIR=${CMAKE_BINARY_DIR}/storage/columnstore/engine-bin
  )

ADD_DEPENDENCIES(columnstore columnstore_engine) 
INSTALL(DIRECTORY engine-bin/${CMAKE_INSTALL_PREFIX}/ DESTINATION .
        USE_SOURCE_PERMISSIONS)

