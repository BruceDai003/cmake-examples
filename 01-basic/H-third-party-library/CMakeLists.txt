cmake_minimum_required(VERSION 3.5)

# Set the project name
project (third_party_include)

message (STATUS "cmake module path:\t${CMAKE_MODULE_PATH}")

# find a boost install with the libraries filesystem and system
find_package(Boost 1.46.1 REQUIRED COMPONENTS filesystem system timer)

# check if boost was found
if(Boost_FOUND)
    message ("boost found")
    message (STATUS "Boost_LIBRARY_DIRS = ${Boost_LIBRARY_DIRS}")
    message (STATUS "Boost_INCLUDE_DIRS = ${Boost_INCLUDE_DIRS}")
    message (STATUS "Boost_LIBRARIES = ${Boost_LIBRARIES}")
    message (STATUS "Boost_VERSION = ${Boost_VERSION}")
    message (STATUS "Boost_FILESYSTEM_LIBRARY = ${Boost_FILESYSTEM_LIBRARY}")
else()
    message (FATAL_ERROR "Cannot find Boost")
endif()

# Add an executable
add_executable(third_party_include main.cpp)

# link against the boost libraries
# 这个是我自己修改过的，必须要在find_package里添加module boost底下的library timer，
# 才能引入这个target Boost::timer，否则找不到会报错。此时Boost_TIMER_LIBRARY这种也被import了。
# 不过没有alias target好用，因为还有它自身的其他link libraries，只要是PUBLIC / INTERFACE类型的，
# 也会被import进来的，比较方便。
target_link_libraries( third_party_include
    PRIVATE
       ${Boost_FILESYSTEM_LIBRARY}
       ${Boost_SYSTEM_LIBRARY}
       ${Boost_TIMER_LIBRARY}
       Boost::timer
)
#target_link_libraries( third_party_include
#    PRIVATE
#        Boost::filesystem
#)
