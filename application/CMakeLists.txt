add_subdirectory(src/modules/a)   # look in module_a subdirectory for CMakeLists.txt to process
add_subdirectory(src/modules/b)   # look in module_a subdirectory for CMakeLists.txt to process

# version 3.11 or later of CMake or needed later for installing GoogleTest
# so let's require it now.
cmake_minimum_required(VERSION 3.11-3.18)

project(main)

message( STATUS "Compiler path: ${CMKE_CXX_CMPILER}" )

#****** mysql connector cpp library ******

# set(FULL_PATH_TO_MYSQL_CONNECTOR_CPP_DIR ../../../libs/mysql-connector-c++-8.0.27-linux-glibc2.12-x86-64bit)
# include_directories(${FULL_PATH_TO_MYSQL_CONNECTOR_CPP_DIR}/include)
# include_directories(${FULL_PATH_TO_MYSQL_CONNECTOR_CPP_DIR}/include/cppconn)
# link_directories(${FULL_PATH_TO_MYSQL_CONNECTOR_CPP_DIR}/lib)

#****** mysql connector cpp library ******

#****** vcpkg integration ******
#include(/home/dsistemas/Desktop/CPP/vcpkg/scripts/buildsystems/vcpkg.cmake)

#****** SQLite ******
#find_package(unofficial-sqlite3 CONFIG REQUIRED)
#find_package(libmysql CONFIG REQUIRED)
#****** SQLite ******

#****** vcpkg integration ******

file(GLOB SOURCES "src/*.cpp")

# file(GLOB OBJS "/home/dsistemas/Desktop/CPP/test04/extern_libs/mysqlpp/static/*.o")

# # SET(OBJS
# #    /home/dsistemas/Desktop/CPP/test04/extern_libs/mysqlpp/static/mysqlpp_connection.o
# # )

# SET_SOURCE_FILES_PROPERTIES(
#   ${OBJS}
#   PROPERTIES
#   EXTERNAL_OBJECT true
#   GENERATED true
# )

#Come from folder extern_libs/mysqlpp/CMakeLists.txt
get_property(MYSQLPP_OBJS GLOBAL PROPERTY MYSQLPP_OBJS )

add_executable(main ${SOURCES} ${MYSQLPP_OBJS})
#add_executable(main src/main.cpp)

target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/includes)

# We need a1.h a2.h b1.h b2.h and the lib_a and lib_b library
target_link_libraries(main PRIVATE module_a)
target_link_libraries(main PRIVATE module_b)

# We need a1.h a2.h b1.h b2.h and the lib_a and lib_b library
target_link_libraries(main PRIVATE lib_a)
target_link_libraries(main PRIVATE lib_b)

#********* libmysqlclient C Driver *********

target_link_libraries(main PRIVATE mysql_client)
target_link_libraries(main PRIVATE mysql_services)

#target_link_libraries(main ${PROJECT_SOURCE_DIR}/extern_libs/mysqlpp/static/mysqlpp_connection.o)
#target_link_libraries(main PRIVATE -L/home/dsistemas/Desktop/CPP/test04/extern_libs/mysqlpp/static/libmysqlpp_ssqls2parse.a)
#target_link_libraries(main PRIVATE mysqlpp_ssqls2parse)
#target_link_libraries(main PRIVATE mysqlpp_excommon)
target_link_libraries(main PRIVATE mysqlpp)

#********* Dependencies from libmysqlclient.a *********

# function(find_static_library LIB_NAME OUT)

#   if (WIN32 OR MSVC)
#      set(CMAKE_FIND_LIBRARY_SUFFIXES ".lib")
#   elseif (UNIX)
#       set(CMAKE_FIND_LIBRARY_SUFFIXES ".a")
#   endif()      

#   find_library(
#       FOUND_${LIB_NAME}_STATIC
#       ${LIB_NAME}
#   )

#     if (FOUND_${LIB_NAME}_STATIC)
#         get_filename_component(ABS_FILE ${FOUND_${LIB_NAME}_STATIC} ABSOLUTE)
#     else()
#         message(SEND_ERROR "Unable to find library ${LIB_NAME}")
#     endif()

#     set(${OUT} ${ABS_FILE} PARENT_SCOPE)

# endfunction()

# find_static_library(ssl SSL)
# find_static_library(crypto CRYPTO)
# find_static_library(pthread PTHREAD)
# find_static_library(c C)
# find_static_library(c_nonshared C_NONSHARED)
# find_static_library(dl DL)
# find_static_library(m M)
# find_static_library(z Z)

#VERY Important made your executable full static linked lik go lang
target_link_options(main PRIVATE --static -static-libgcc -static-libstdc++)

target_link_libraries(main PRIVATE ssl)
target_link_libraries(main PRIVATE crypto)
target_link_libraries(main PRIVATE pthread)
target_link_libraries(main PRIVATE dl)
target_link_libraries(main PRIVATE m)
target_link_libraries(main PRIVATE z)
#target_link_libraries(main PRIVATE ${C_NONSHARED})
#target_link_libraries(main PRIVATE ${C})
#********* Dependencies from libmysqlclient.a *********

#********* libmysqlclient C Driver *********

#********* libmysql++ C++ Class *********

# set_target_properties(main PROPERTIES LINK_SEARCH_START_STATIC 1)
# set_target_properties(main PROPERTIES LINK_SEARCH_END_STATIC 1)
# set(CMAKE_FIND_LIBRARY_SUFFIXES ".a")

# target_link_libraries(main PRIVATE SOMELIB1)
# target_link_libraries(main PRIVATE SOMELIB2)

#********* libmysql++ C++ Class *********

#****** mysql connector cpp library ******

# target_link_libraries(main PRIVATE mysqlcppconn)

#****** mysql connector cpp library ******

#****** vcpkg integration ******

#****** SQLite ******
#target_link_libraries(main PRIVATE unofficial::sqlite3::sqlite3)
#target_link_libraries(main PRIVATE ${MYSQL_LIBRARIES})
#****** SQLite ******

#****** vcpkg integration ******

#-static-libgcc -static-libstdc++

# Tell C++ compiler to use C++20 features. We don't actually use any of them.
target_compile_features(main PUBLIC cxx_std_20)