cmake_minimum_required(VERSION 2.8.9)
project(AccountServer)
SET(PROJECT_ROOT_PATH ".")
include_directories(./ ../ServerEngine /usr/include/lua5.3 ../ServerEngine/DBInterface ../ServerData /usr/include/mysql ../../Common/protobuf/src ../Message ../LogData ../PublicData ../WatcherClient)
link_directories(../../Common/lib /usr/lib64/mysql) 


#消息
set(PROTO Proto)
FILE(GLOB PROTO "../Message/Msg_Game.pb.cc" 
                "../Message/Msg_Game.pb.h"
                "../Message/Msg_Account.pb.cc"
                "../Message/Msg_Account.pb.h"
                "../Message/Msg_Base.pb.cc"
                "../Message/Msg_Base.pb.h"
                "../Message/Msg_ID.pb.cc"
                "../Message/Msg_ID.pb.h")


#header file code
set(HEADER_CODE_FILE HeaderFiles)
FILE(GLOB HEADER_CODE_FILE "./*.h")


#source file code
set(SOURCE_CODE_FILE SourceFiles)
FILE(GLOB SOURCE_CODE_FILE "./*.cpp")

#Watcher Client
set(WATCHER_CLIENT WatcherClient)
FILE(GLOB WATCHER_CLIENT "../WatcherClient/WatcherClient.cpp" "../WatcherClient/WatcherClient.h")

source_group(Proto FILES ${PROTO} )
source_group(HeaderFiles FILES ${HEADER_CODE_FILE} )
source_group(SourceFiles FILES ${SOURCE_CODE_FILE} )
source_group(WatcherClient FILES ${WATCHER_CLIENT} )



# Target
add_executable(AccountServer
    ${PROTO}    
    ${WATCHER_CLIENT}   
    ${HEADER_CODE_FILE} 
    ${SOURCE_CODE_FILE} 
)


if(MSVC)
    target_link_libraries(AccountServer ServerEngine protobuf libmysql ws2_32)
    ADD_DEFINITIONS(-D_CONSOLE -D_CRT_SECURE_NO_WARNINGS)
else()
    target_link_libraries(AccountServer ServerEngine protobuf mysqlclient pthread )
    ADD_DEFINITIONS(-D_CONSOLE -D_CRT_SECURE_NO_WARNINGS)
endif()


if(CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall -pthread -std=c++11")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -Wall -pthread -std=c++11")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -w -O3 -std=c++11")
endif()

