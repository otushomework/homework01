cmake_minimum_required(VERSION 3.2)

#проект
project(helloworld VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})

#поиск пакетов-зависимостей
find_package(Boost COMPONENTS unit_test_framework REQUIRED)

#препроцессор собирающий файл version.h
configure_file(version.h.in ${PROJECT_SOURCE_DIR}/version.h)

#сборка исполняемого файла
add_executable(${PROJECT_NAME} main.cpp)

#сборка библиотеки
add_library(${PROJECT_NAME}_lib lib.cpp lib.h)

#сборка теста
add_executable(test_version test_version.cpp)

#задаем параметры компилятора
set_target_properties(${PROJECT_NAME} test_version PROPERTIES
  CXX_STANDARD 14
  CXX_STANDARD_REQUIRED ON
  COMPILE_OPTIONS "-Wpedantic;-Wall;-Wextra"
)

#задаем параметры линковки
set_target_properties(test_version PROPERTIES
  COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
  INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)

#линкуем библиотеку к приложению
target_link_libraries(${PROJECT_NAME} ${PROJECT_NAME}_lib)

#линкуем библиотеку к тестам
target_link_libraries(test_version
  ${Boost_LIBRARIES}
  ${PROJECT_NAME}_lib
)


#куда закидывать cli после установки готового пакета
install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)

#задаем версию в пакете
set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT v31337@gmail.com)

#выбираем генератор пакета
set(CPACK_GENERATOR DEB)

set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)

include (CPack)

enable_testing()

add_test(test_version_valid test_version)
