
configure_file(CMakeLists.txt.in googletest-download/CMakeLists.txt)
execute_process(COMMAND "${CMAKE_COMMAND}" -G "${CMAKE_GENERATOR}" .
    WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/googletest-download")
execute_process(COMMAND "${CMAKE_COMMAND}" --build .
    WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/googletest-download" )

set(GTEST_DIR "${CMAKE_BINARY_DIR}/googletest-src/googletest" CACHE PATH "Root directory for GoogleTest")

add_executable(tests test.cpp tests-parsing.cpp ${GTEST_DIR}/src/gtest-all.cc ${GTEST_DIR}/src/gtest_main.cc)
add_dependencies(tests json-toolkit)
target_include_directories(tests PUBLIC "${GTEST_DIR}/include")
target_include_directories(tests PRIVATE "${GTEST_DIR}")
target_include_directories(tests PUBLIC "../include")

if (NOT DEFINED WIN32)
  target_link_libraries(tests pthread)
endif()

add_test(libjsontests tests)