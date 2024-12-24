cmake_minimum_required(VERSION 3.10)
cmake_policy(SET CMP0167 NEW)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})

project(lab7 VERSION ${PROJECT_VERSION})

add_executable(lab7 main.cpp)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")    # Флаг для компилятора MSVC (Windows)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /std:c++17")
    else()
    # Флаг для g++ и clang++ (Linux, MacOS)    
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")
endif()

install(TARGETS lab7 DESTINATION bin)

set(CPACK_GENERATOR "ZIP")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")
set(CPACK_PACKAGE_NAME "lab7")
set(CPACK_PACKAGE_DESCRIPTION "Описание вашего проекта")
set(CPACK_PACKAGE_VENDOR "Ваше имя или компания")
set(CPACK_PACKAGE_CONTACT "Ваш контактный email")

include(CPack)

set(Boost_COMPILER "vc142")  # Укажите компилятор Visual Studio
set(BOOST_DIR "C:/libs/boost_1_80_0/stage/lib/cmake/Boost-1.80.0")  # Путь к каталогу, где лежат BoostConfig.cmake
#set(BOOST_HEADERS "C:/libs/boost_1_80_0/stage/lib/cmake/boost_headers-1.80.0") 

find_package(Boost REQUIRED COMPONENTS filesystem program_options PATHS ${BOOST_DIR})
# Находим Boost и проверяем, что он найден
find_package(Boost REQUIRED COMPONENTS filesystem program_options)

# Проверяем, что Boost найден успешно
if(Boost_FOUND)
 # Добавляем необходимые include директории
 target_include_directories(lab7 PRIVATE ${Boost_INCLUDE_DIRS})
 # Линкуем необходимые библиотеки Boost
 target_link_libraries(lab7 PRIVATE ${Boost_LIBRARIES})
else()
 message(FATAL_ERROR "Boost not found! Please install Boost and make sure it's in your system's include and library paths.")
endif()