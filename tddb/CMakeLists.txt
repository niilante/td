cmake_minimum_required(VERSION 3.0.2 FATAL_ERROR)

#SOURCE SETS
set(TDDB_SOURCE
  td/db/binlog/Binlog.cpp
  td/db/binlog/BinlogEvent.cpp
  td/db/binlog/ConcurrentBinlog.cpp
  td/db/binlog/detail/BinlogEventsBuffer.cpp
  td/db/binlog/detail/BinlogEventsProcessor.cpp

  td/db/SqliteDb.cpp
  td/db/SqliteStatement.cpp
  td/db/SqliteKeyValueAsync.cpp

  td/db/detail/RawSqliteDb.cpp

  td/db/binlog/Binlog.h
  td/db/binlog/BinlogInterface.h
  td/db/binlog/BinlogEvent.h
  td/db/binlog/BinlogHelper.h
  td/db/binlog/ConcurrentBinlog.h
  td/db/binlog/detail/BinlogEventsBuffer.h
  td/db/binlog/detail/BinlogEventsProcessor.h

  td/db/BinlogKeyValue.h
  td/db/DbKey.h
  td/db/KeyValueSyncInterface.h
  td/db/Pmc.h
  td/db/SeqKeyValue.h
  td/db/SqliteConnectionSafe.h
  td/db/SqliteDb.h
  td/db/SqliteKeyValue.h
  td/db/SqliteKeyValueAsync.h
  td/db/SqliteKeyValueSafe.h
  td/db/SqliteStatement.h
  td/db/TsSeqKeyValue.h

  td/db/detail/RawSqliteDb.h
)

add_library(tddb STATIC ${TDDB_SOURCE})
target_include_directories(tddb PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
target_link_libraries(tddb PUBLIC tdactor tdutils PRIVATE tdsqlite)

if (NOT CMAKE_CROSSCOMPILING)
  add_executable(binlog_dump td/db/binlog/binlog_dump.cpp)
  target_link_libraries(binlog_dump PRIVATE tddb)
endif()

install(TARGETS tddb EXPORT TdTargets
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)
