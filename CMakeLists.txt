cmake_minimum_required(VERSION 3.12)
project(ENCRYPTO_utils LANGUAGES CXX)

if (CMAKE_COMPILER_IS_GNUCXX AND CMAKE_CXX_COMPILER_VERSION VERSION_LESS 8.0)
    message(FATAL_ERROR "ENCRYPTO_utils require at least g++-8")
endif()

option(ENCRYPTO_UTILS_BUILD_TESTS "Build tests" Off)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

find_package(GMP REQUIRED)
find_package(GMPXX REQUIRED)
find_package(OpenSSL REQUIRED)
find_package(Boost 1.66.0 REQUIRED COMPONENTS system thread)

set(ecclvl 251) #set this for the ecc security level

#relic library configuration
set(ALIGN 16)
set(DARCH X64)
#set(ARITH curve2251-sse)
set(CHECK off)
set(FB_POLYN ${ecclvl})
set(FB_METHD "INTEG;INTEG;QUICK;QUICK;QUICK;QUICK;LOWER;SLIDE;QUICK")
set(FB_PRECO on)
set(FB_SQRTF off)
set(EB_METHD "PROJC;LODAH;COMBS;INTER")
set(EC_METHD "CHAR2")
set(COMP "-O3 -funroll-loops -fomit-frame-pointer -march=core2 -mssse3")
set(TIMER CYCLE)
set(WITH "MD;DV;BN;FB;EB;EC")
set(WSIZE 64)
set(TESTS 0)
set(BENCH 0)
set(SHLIB off)

#TODO fix this to a proper include of the headers, it just works, but it looks dirty
include_directories(${CMAKE_SOURCE_DIR}/extern/relic/include ${CMAKE_SOURCE_DIR}/extern/relic/include/low ${CMAKE_BINARY_DIR}/extern/relic/include)
add_subdirectory(extern/relic)

#TODO fix this to a proper preprocessor definition, it just works, but it looks dirty
add_compile_definitions(ECCLVL=${ecclvl})

add_subdirectory(src)

if(ENCRYPTO_UTILS_BUILD_TESTS)
	add_subdirectory(extern/googletest EXCLUDE_FROM_ALL)
	add_subdirectory(test)
endif()
