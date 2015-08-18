# Test cmake for debugging of issues found with wdt and cmake

cmake_minimum_required(VERSION 3.3)

# There is no C per se in WDT but if you use CXX only here many checks fail
# Version is Major.Minor.YYMMDDX for up to 10 releases per day
# Minor currently is also the protocol version - has to match with Protocol.cpp
project("Test1" LANGUAGES C CXX VERSION 1.15.1507290)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED on)

# What we need to build the part of folly we use:

include(CheckIncludeFileCXX)
include(CheckFunctionExists)
include(CheckLibraryExists)
include(CheckCXXSourceCompiles)
# For WDT itself:
set(CMAKE_REQUIRED_DEFINITIONS ${CMAKE_CXX11_STANDARD_COMPILE_OPTION})
check_function_exists(posix_fallocate HAS_POSIX_FALLOCATE)
check_function_exists(sync_file_range HAS_SYNC_FILE_RANGE)
check_function_exists(posix_memalign HAS_POSIX_MEMALIGN)
check_function_exists(setsockopt HAS_SETSOCKOPT)
check_function_exists(open HAS_OPEN)
# C based check (which fail with the c++ setting thereafter...)
#check_library_exists(rt clock_gettime "" FOLLY_HAVE_CLOCK_GETTIME)
#set(SAVE_CMRL ${CMAKE_REQUIRED_LIBRARIES}) #globals are evil/ugly
#set(CMAKE_REQUIRED_LIBRARIES ${CMAKE_THREAD_LIBS_INIT})
#check_function_exists(pthread_atfork FOLLY_HAVE_PTHREAD_ATFORK)
#set(CMAKE_REQUIRED_LIBRARIES ${SAVE_CMRL}) #globals are evil/ugly
# Needed until Cmake issue #15361 is addressed
#set(CMAKE_REQUIRED_DEFINITIONS ${CMAKE_CXX11_STANDARD_COMPILE_OPTION})
set(CMAKE_REQUIRED_DEFINITIONS ${CMAKE_CXX11_STANDARD_COMPILE_OPTION})
#check_include_file_cxx(malloc.h FOLLY_HAVE_MALLOC_H)
#check_include_file_cxx(features.h FOLLY_HAVE_FEATURES_H)
#check_include_file_cxx(bits/c++config.h FOLLY_HAVE_BITS_CXXCONFIG_H)
#check_include_file_cxx(bits/functexcept.h FOLLY_HAVE_BITS_FUNCTEXCEPT)
#check_function_exists(clock_gettime FOLLY_HAVE_CLOCK_GETTIME)
#check_cxx_source_compiles("#include <type_traits>
#      #if !_LIBCPP_VERSION
#      #error No libc++
#      #endif
#      int main() {return 0;}" FOLLY_USE_LIBCPP)
#check_cxx_source_compiles(
#"extern \"C\" void cmkcheckweak() __attribute__((weak));
#int main(int argc, char** argv) {
#  return (cmkcheckweak) ? 1 : 0 ;
#}" FOLLY_HAVE_WEAK_SYMBOLS)

configure_file(test.h.in test.h)
