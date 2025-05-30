cmake_minimum_required(VERSION 3.10)
project(SearchWord C)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -O2")

add_executable(basic basic.c)
add_executable(ignore ignore.c)
add_executable(nmap nmap.c)

add_custom_target(setup
    COMMAND ${CMAKE_COMMAND} -E env bash ./gen_structure.sh
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    COMMENT "Running setup script"
)

add_custom_target(run
    DEPENDS basic ignore nmap setup
    COMMAND ${CMAKE_COMMAND} -E echo "[search_basic] поиск слова"
    COMMAND ./basic test
    COMMAND ${CMAKE_COMMAND} -E echo "[search_ignore] поиск слова с -i"
    COMMAND ./ignore -i test
    COMMAND ${CMAKE_COMMAND} -E echo "[search_mmap] поиск слова через mmap"
    COMMAND ./nmap -i test
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)

add_custom_target(clean-all
    COMMAND ${CMAKE_COMMAND} -E rm -f basic ignore nmap
    COMMENT "Cleaning all binaries"
)