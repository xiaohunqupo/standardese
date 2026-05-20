# Copyright (C) 2016-2017 Jonathan Müller <jonathanmueller.dev@gmail.com>
# This file is subject to the license terms in the LICENSE file
# found in the top-level directory of this distribution.

set(header filesystem.hpp generator.hpp thread_pool.hpp)
set(src generator.cpp main.cpp)

add_executable(standardese_tool ${header} ${src})
target_link_libraries(standardese_tool PRIVATE standardese)
target_include_directories(standardese_tool PRIVATE ${THREADPOOL_INCLUDE_DIR})
set_target_properties(standardese_tool PROPERTIES OUTPUT_NAME standardese CXX_STANDARD 17)

# link Boost
find_package(Boost COMPONENTS program_options filesystem REQUIRED)
if(Boost_MAJOR_VERSION EQUAL 1 AND Boost_MINOR_VERSION LESS 69)
    find_package(Boost COMPONENTS program_options filesystem system REQUIRED)
endif()
target_include_directories(standardese_tool PRIVATE ${Boost_INCLUDE_DIR})
target_link_libraries(standardese_tool PRIVATE ${Boost_LIBRARIES})

# install standardese binary
install(TARGETS standardese_tool EXPORT standardese DESTINATION "${tool_dest}")
